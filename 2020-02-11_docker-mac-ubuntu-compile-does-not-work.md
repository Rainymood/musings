# Node modules compiled on Mac OSX do not work on Linux

Trying to create a hot reload Docker workflow but running into this error

```bash
container@02643333d268:/app$ nodemon index.js
[nodemon] 2.0.2
[nodemon] to restart at any time, enter `rs`
[nodemon] watching dir(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node index.js`
internal/modules/cjs/loader.js:805
  return process.dlopen(module, path.toNamespacedPath(filename));
                 ^

Error: /app/node_modules/@tensorflow/tfjs-node/lib/napi-v4/tfjs_binding.node: invalid ELF header
    at Object.Module._extensions..node (internal/modules/cjs/loader.js:805:18)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Module.require (internal/modules/cjs/loader.js:690:17)
    at require (internal/modules/cjs/helpers.js:25:18)
    at Object.<anonymous> (/app/node_modules/@tensorflow/tfjs-node/dist/index.js:44:16)
    at Module._compile (internal/modules/cjs/loader.js:776:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:787:10)
    at Module.load (internal/modules/cjs/loader.js:653:32)
[nodemon] app crashed - waiting for file changes before starting...
```

According to
[this](https://stackoverflow.com/questions/15809611/bcrypt-invalid-elf-header-when-running-node-app)
stackoverflow post `bcrypt` compiled on OSX does not work on Linux.

>I've found that bcrypt compiled on OSX will not quite work on Linux. In other
words, if you check in the bcrypt compiled on your local OSX workstation, and
try to run the node app on your linux servers, you will see the error above.

>Solution: npm install bcrypt on Linux, check that in, solved.

>Probably the best way to deal with this is exclude your node_modules in
.gitignore... and npm install remotely.

In other words, it makes a lot of sense that `node.js` modules compiled on
OSX do not work on Linux/Ubuntu. Bummer. 