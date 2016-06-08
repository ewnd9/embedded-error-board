# embedded-error-board

> Track and fix JavaScript errors fired by your visitor's browsers.

WIP fork of https://github.com/Lapple/ErrorBoard

## Install

```sh
$ npm install embedded-error-board --save
```

## Usage

- [`express-example/index.js`](example/express-app/index.js)

```js
const express = require('express');

const app = express();

const mount = '/error-board';
const dbFile = __dirname + '/errors.db';

const errorBoard = require('embedded-error-board')(dbFile, mount);
app.use(mount, errorBoard.app);

const server = http.createServer(app);
errorBoard.ws.installHandlers(server, { prefix: `${mount}/ws` });

app.use(function(err, req, res, next) { // always last
  if (!err) {
    return next();
  }

  console.log(err);

  errorBoard.agent.report(err, { family: 'backend' });
  res.status(err.status || 500).json({ status: 'error' });
});

server.listen(port);
console.log('Listening on port %s', port);
```


## Development

```sh
$ npm run start:dev
```

## License

MIT © [ewnd9](http://ewnd9.com)
