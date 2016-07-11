# atomic-batcher

A simple batching function that allows you to atomically batch a series of operations.

```
npm install atomic-batcher
```

## Usage

``` js
var batcher = require('atomic-batcher')
var db = require('level')('some.db')

var batch = batcher(function work (ops, cb) {
  // only one batch will happen at the time
  console.log('batching', ops)
  db.batch(ops, cb)
})

batch({type: 'put', key: 'hello', value: 'world-1'})
batch({type: 'put', key: 'hello', value: 'world-2'})
batch({type: 'put', key: 'hello', value: 'world-3'})
batch({type: 'put', key: 'hi', value: 'hello'}, function () {
  db.get('hello', console.log) // returns world-3
  db.get('hi', console.log) // returns hello
})
```

## API

#### `var batch = batcher(worker)`

Create a new batching function. `worker` should be a function that accepts a batch and a callback, `(batch, cb)`.
Only one batch is guaranteed to be run at the time.

The `batch` function accepts a value or an array of values and a callback, `batch(value(s), cb)`. The callback is called when the batch containing the values have been run.

## License

MIT
