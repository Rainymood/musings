# How to run a single test in Jest

```bash
npm test -- -t "test foo"
```

From
[here](https://stackoverflow.com/questions/44446626/run-only-one-test-with-jest?rq=1).
Make sure to add the `--`, for some reason we need it.

Compare without

```bash
➜ npm test -t "test foo" 

> project@3.5.43 test /Users/jan/Documents/Projects/api
> jest --silent --coverage --testPathIgnorePatterns ./test/integration "test foo"
```

and with

```bash
➜ npm test -- -t "test foo" 

> project@3.5.43 test /Users/jan/Documents/Projects/api
> jest --silent --coverage --testPathIgnorePatterns ./test/integration "-t" "test foo"
```

with our `package.json`

```JS
{
  },
  "scripts": {
    "test": "jest --silent --coverage --testPathIgnorePatterns ./test/integration",
    ...
  },
}

```