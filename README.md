# thread-stream
[![npm version](https://img.shields.io/npm/v/thread-stream)](https://www.npmjs.com/package/thread-stream)
[![Build Status](https://img.shields.io/github/workflow/status/pinojs/thread-stream/CI)](https://github.com/pinojs/thread-stream/actions)
[![Known Vulnerabilities](https://snyk.io/test/github/pinojs/thread-stream/badge.svg)](https://snyk.io/test/github/pinojs/thread-stream)
[![Coverage Status](https://coveralls.io/repos/github/pinojs/thread-stream/badge.svg?branch=master)](https://coveralls.io/github/pinojs/thread-stream?branch=master)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://standardjs.com/)

A streaming way to send data to a Node.js Worker Thread.

## install

```sh
npm i thread-stream
```

## Usage

```js
'use strict'

const ThreadStream = require('thread-stream')
const { join } = require('path')

const stream = new ThreadStream({
  filename: join(__dirname, 'worker.js'),
  workerData: { dest },
  workerOpts: {}, // Other options to be passed to Worker
  sync: false, // default
})

stream.write('hello')

// Asynchronous flushing
stream.flush(function () {
  stream.write(' ')
  stream.write('world')

  // Synchronous flushing
  stream.flushSync()
  stream.end()
})
```

In `worker.js`:

```js
'use strict'

const fs = require('fs')
const { once } = require('events')

async function run (opts) {
  const stream = fs.createWriteStream(opts.dest)
  await once(stream, 'open')
  return stream
}

module.exports = run
```

## License

MIT
