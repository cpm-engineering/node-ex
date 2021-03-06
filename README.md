# node-ex
An opinionated NodeJS application that uses [ExpressJS](https://expressjs.com/).  

_The structure of this application reflects the easiest way the author found to
get started with a js application and could be lacking or not reflect best
practices_.

Some other, more comprehensive, NodeJS examples can be found on
[Github](https://github.com/mschwarzmueller/nodejs-basics-tutorial).  

#### Getting started  

First you need to install [yarn](https://yarnpkg.com/en/docs/install).


Install all dependencies for "production".
```sh
yarn install
```
`port` and `host` bindings can be configured by creating a `.env` file in the project root.
This file will get loaded at application init.  
e.g.:
```sh
PORT=3030
HOST=127.0.0.1
```

Run the application  
```sh
$ yarn run v1.3.2
$ NODE_PATH=./config:./routes:./controllers:./models:./assets node server.js
Application accessible on http://127.0.0.1:3030
```

##### Development

Installing the `dev` dependencies:
```sh
yarn install --dev
```

Run the dev server with hot-reload using [supervisor](https://www.npmjs.com/package/supervisor).  
Supervisor will monitor for changes in `.html` or `.js` files.

```sh
$ yarn devserve
yarn run v1.3.2
$ NODE_PATH=./config:./routes:./controllers:./models:./assets ./node_modules/supervisor/lib/cli-wrapper.js -e 'html|js' app server.js

Running node-supervisor with
  program 'server.js'
  --watch '.'
  --extensions 'html|js'
  --exec 'node'

Starting child process with 'node server.js'
Watching directory '/$HOME/$USER/cpm-engineering/node-ex' for changes.
Press rs for restarting the process.
Application accessible on http://127.0.0.1:3030
```

#### Observations

We set the `NODE_PATH` env var, in `package.json`, to tell Node where to find
our modules. Although it's more obscure it allows us to use modules globally and
not have to do relative imports e.g.: `const config = require('./config')`.

```json
"scripts": {
  "test": "NODE_PATH=./config:./routes:./models:./assets ./node_modules/mocha/bin/mocha --timeout 500 -R spec --ui tdd tests/*_test.js",
  "start": "NODE_PATH=./config:./routes:./models:./assets node server.js",
  "devserve": "NODE_PATH=./config:./routes:./models:./assets ./node_modules/supervisor/lib/cli-wrapper.js -e 'html|js' app server.js"
}
```


#### Testing

This will run a server and test for the endpoints defined.

```sh
$ yarn run test
yarn run v1.3.2
$ NODE_PATH=./config:./routes:./models:./assets ./node_modules/mocha/bin/mocha --timeout 500 -R spec --ui tdd tests/*_test.js

  Basic application endpoint test
GET / 200 16.093 ms - 2453
    ✓ GET to index should return 200
GET /healthz 200 0.322 ms - 2
    ✓ GET to /healthz should return 200


  2 passing (53ms)

✨  Done in 0.68s.
```
