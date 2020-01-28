# Debugging

In this musing I want to take you through my debugging process. It's not
perfect, but it's mine.

In doing so I hope to provide more clarity of thinking to myself and to
expose my mental models to the light of day, in hopes of improving them.

I actually wrote this document **while** solving the bug, and in the end I
did manage to solve it, so I guess it helped :).

## The problem

Recently I got smacked in the face with this error message.

```bash
{"application-name":"nl-tara-localhost","BusinessProcessId":"v2session","level":"ERROR","log-id":52,"message":{"msg":"Could not send messages to Chatbase: Error: One or more required fields were not set on the message, please check the documentation on how to set the following message fields:\napi_key, type, user_id, time_stamp, platform, message"},"type":"chatbase"}
```

The error message quite literally asks me to check out `api_key, type,
user_id, time_stamp, platform, message`. To do so I added some console logs.

```JS
const set = this.chatbase.newMessageSet()
    .setPlatform(this.platform)
    ...
    .setIntent(intent);
console.log(set);
```

Which resulted in something like this.

```JS
Logging to chatbase
MessageSet {
  api_key: 'XXX-XXX-XXX',
  user_id: XXX-XXX-XXX,
  platform: 'app-localhost',
  type: null,
  version: '3.0',
  intent: 'intent',
  session_id: 'session_id',
  transport_timeout: 5000,
  messages: [],
  _state:
   MessageLifecycleState { _state: { create: [Object], update: [Object] } } }
```

Indeed, we see that in `MessageSet` that `type: null`. Could this be the root cause of the error? 

```JS
const msg = set.newMessage()
    .setMessage(userMessage)
    .setMessageId(messageId)
    .setTimestamp(timestamp)
    .setAsTypeUser();
console.log(msg);
```

Perhaps further down in the program, in the `MessageSetMessage` there could be another clue, so let's investigate. 

```JS
MessageSetMessage {
  api_key: 'XXX-XXX-XXX',
  user_id: XXX-XXX-XXX,
  type: 'user',
  time_stamp: '1580200355907',
  platform: 'app-localhost',
  message: 'message',
  intent: 'intent',
  not_handled: false,
  feedback: false,
  version: '3.0',
  message_id: 'msg_id',
  response_time: null,
  session_id: 'session_id',
  _state:
   MessageLifecycleState { _state: { create: [Object], update: [Object] } },
  transport_timeout: 5000 }
```

This looks OK? Where is the bug hiding then? What is exactly the thing that triggers the bug? 

```JS
return set.sendMessageSet()
    .catch(error => {
        logger.error('chatbase', { msg: `Could not send messages to Chatbase: ${error}`}, messageId);
    });
```

Interesting...

If we log the `messageSet` we find

```JS
MessageSet {
  ...
  type: null,
  ...
}
 ```

Then after adding the human and bot message

```JS
MessageSet {
  ...
  type: null,
  ...
  messages:
   [ MessageSetMessage {
       ...
       type: 'user',
       ...
     },
     MessageSetMessage {
       type: 'agent',
     }
   ]
```

So, perhaps its about the MessageSet `type` being `null`? 

## Attempt #1

This leads me to my first hypothesis. The hypothesis is that the bug is being caused by `type: null` in `messageSet`. To test this hypothesis, I added this 

```JS
const set = this.chatbase.newMessageSet()
    .setPlatform(this.platform)
    ...
    .setIntent(intent)
    .setAsTypeUser(); // new
```

*5 minutes later*

Nope, that didn't work. 

## Attempt #2 Try to write a small reproducable script

Can I write a small script that reproduces the error? 

```JS
'use strict';

const chatbase = require('@google/chatbase');
const config = require('./src/config');

const version = 3;
const userId = '...';
const sessionId = '...';
const messageId = '...';
const intent = '...'
const userMessage = '...';
const timestamp = '1580201613022';
const botResponse = 'botresponse'
const responseTimestamp = '1580201614181';

class ChatbaseSingleton {
    constructor() {
        this.chatbase = chatbase;
        this.platform = 'app-localhost';
    }

    log(version, userId, sessionId, messageId, intent, userMessage, timestamp, botResponse, responseTimestamp) {
        console.log("Logging to chatbase");
        const botError = intent === 'matching_error' || intent === 'contact_human_agent';
        const set = this.chatbase.newMessageSet()
            .setPlatform(this.platform)
            .setVersion(String(version).concat('.0'))
            .setApiKey(config.chatbase.api_key)
            .setUserId(userId)
            .setCustomSessionId(sessionId)
            .setIntent(intent)
            .setAsTypeUser(); // new 
        console.log("=== - this.chatbase.newMessageSet() ===")
        console.log(set);

        // - Add user message
        const msg = set.newMessage()
            .setMessage(userMessage)
            .setMessageId(messageId)
            .setTimestamp(timestamp)
            .setAsTypeUser();
        console.log("=== - Add user message ===")
        console.log(msg)

        // - Set handled status
        if(!botError) {
            msg.setAsHandled();
        } else {
            msg.setAsNotHandled();
        }
        console.log("=== - Set handled status ===")
        console.log(msg)

        // - Add Watson message
        set.newMessage()
            .setMessage(botResponse)
            .setMessageId(messageId)
            .setTimestamp(responseTimestamp)
            .setAsTypeAgent();
        console.log("=== - Add Watson message ===")
        console.log(set)

        return set.sendMessageSet()
            .catch(error => {
                logger.error('chatbase', { msg: `Could not send messages to Chatbase: ${error}`}, messageId);
            });
    }
}

const C = new ChatbaseSingleton;
C.log(version, userId, sessionId, messageId, intent, userMessage, timestamp, botResponse, responseTimestamp);
```

After running this, I actually found out that this does work!

## Attempt #3 Perhaps ... networking? 

Then I thought that perhaps the code wasn't the issue, but the networking. To
check this, I exec'd into the container and checked out the hardcoded URLs in
the chatbase-node project in the `node_modules` folder.

```
docker exec -it <container id>
$ cat node_modules/@google/chatbase/lib/Transport.js
```

Results in 

```JS
const CREATE_ENDPOINT = 'http://chatbase.outbound-gateway...';
const CREATE_SET_ENDPOINT = 'http://chatbase.outbound-gateway...';
const UPDATE_ENDPOINT = 'http://chatbase.outbound-gateway...';
```

The reason I check these is because we are changing these in our `Dockerfile`
because we are running our service behind a reverse proxy and do not allow
any external `https` connections to the outside world.

To test this, I manually changed the URLs in my local `node_modules` and ran the small script we made above. 

```JS
// const CREATE_ENDPOINT = 'https://chatbase-area120.appspot.com/api/message';
// const CREATE_SET_ENDPOINT = 'https://chatbase-area120.appspot.com/api/messages';
// const UPDATE_ENDPOINT = 'https://chatbase-area120.appspot.com/api/message/update';
const CREATE_ENDPOINT = 'http://chatbase.outbound-gateway...';
const CREATE_SET_ENDPOINT = 'http://chatbase.outbound-gateway...';
const UPDATE_ENDPOINT = 'http://chatbase.outbound-gateway...';
```

Running the small script locally with these URLs also seemed to work. With this we can rule out any networking issues, or the most common ones that I've faced before. 

## Attempt #4 Search the internet

I then searched the internet to see if I could find a minimum viable curl
command that I could send to our chatbase API to get some response. I found
one
[here](https://codelabs.developers.google.com/codelabs/chatbase/index.html?index=..%2F..index#2).

```bash
➜ curl -X POST \
  https://chatbase-area120.appspot.com/api/message \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{
  "api_key": "...",
  "type": "user",
  "user_id": "1234",
  "platform": "gorge-of-peril",
  "message": "Hello world",
  "not_handled": false
}'
{"message_id": "27795696600", "status": 200}
```

This also seems to work?! 

## The solution

When I saw this curl request, it suddenly hit me. If you scroll up you see
that `user_id` is a string here, and **not** a string in all the requests
above.

```JS
MessageSet {
  ..
  user_id: 31640407040,
  ...
}
```

To verify this I went in the `node_modules/@google/chatbase` code. And
indeed, the `user_id` is supposed to be a string.

```JS
  /**
   * Set the user id field for all new messages produced by this interface
   * @function setUserId
   * @param {String} userId - the user id for this client instance
   * @chainable
   */
  setUserId (userId) {
    this.user_id = userId;
    return this;
  }
```

*5 minutes later*

**🎉 THIS FIXED IT! 🎉**

Knowing the root cause, fixing it is easy by calling the `toString()` method
to make sure all `user_id`s are strings, and even if they are not they are
now being typecast as one.

## What can we learn from this?

* Good debug messages are really high leverage providing you with the right info to debug.
* Validate input from sources and throw errors if they are of formats you don't expect.
* Write small scripts that can reproduce the bug on their own, this improves the debugging workflow by speeding it up. 