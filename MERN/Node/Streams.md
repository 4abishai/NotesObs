### ReadStream
First make a big file
```js
let { writeFileSync } = require('fs')

for (let i = 0; i < 1000; i++) {
    writeFileSync(
        '/home/abishai/Documents/node/content/big.txt',
        `written at i=${i}\n`,
        { flag: 'a' }
    )
}
```

```js
const { createReadStream } = require('fs')
let stream = createReadStream('/home/abishai/Documents/node/content/big.txt')

stream.on('data', (res) => {
    console.log(res);
})
```

`createReadStream` returns a readable stream object that you can use to read data from a file. This stream object is an instance of the `ReadStream` class in Node.js's `fs` module.

- `ReadStream` refers to an object created by calling `fs.createReadStream()` to read from a file. `ReadStream` is an instance of the `fs.ReadStream` class, which inherits from `stream.Readable`.

- `Readable` refers to the class itself. `stream.Readable` is the base class for all readable streams in Node.js, including `fs.ReadStream` instances
	Class: Readable; Inherits: stream EventEmitter

 This means that `ReadStream` objects can emit events, such as `data`, `end`, and `error`, which can be listened to using the `.on` method or other event handling mechanisms provided by Node.js.

#### Buffer
The `data` event in Node.js streams is emitted whenever the stream is passing a chunk of data to the consumer. By default, if no encoding is set, the data is emitted as a `Buffer` object.

```js
const { createReadStream } = require('fs');

let stream = createReadStream('/home/abishai/Documents/node/content/jap.txt');

stream.on('data', (res) => {
    console.log(res)
    console.log('Number of Bytes:', res.length)
});
```

```css
<Buffer e3 81 93 e3 82 93 e3 81 ab e3 81 a1 e3 81 af>
Number of Bytes: 15
```

The `res` parameter in the `data` event callback is a `Buffer` object, so `res.length` gives the number of bytes in the buffer, not the number of characters.
#### String
However, if you set an encoding using the `setEncoding()` method on a readable stream, the data will be converted to a string in that encoding before being emitted.

```js
const { createReadStream } = require('fs');

let stream = createReadStream('/home/abishai/Documents/node/content/jap.txt', 'utf-8');
// stream.setEncoding('utf-8'); // alternative

stream.on('data', (res) => {
    console.log(res)
    console.log('Number of characters:', res.length)
});
```

```css
こんにちは
Number of characters: 5
```

#### Option: HighWaterMark
The `highWaterMark` option is used in Node.js streams to specify the maximum amount of data (in bytes) that can be read from a readable stream or written to a writable stream in a single operation.

The amount of data potentially buffered depends on the `highWaterMark` option passed into the stream's constructor. For normal streams, the `highWaterMark` option specifies a [total number of bytes](https://nodejs.org/api/stream.html#highwatermark-discrepancy-after-calling-readablesetencoding). For streams operating in object mode, the `highWaterMark` specifies a total number of objects.

```css
const { createReadStream } = require('fs');

let stream = createReadStream('/home/abishai/Documents/node/content/big.txt', { highWaterMark: 1024, encoding: 'utf-8' });

stream.on('data', (chunk) => {
    console.log(`Received ${chunk.length} bytes of data.`);
});
```

#### Event: Error
In Node.js streams, the `'error'` event is emitted when an error occurs during the processing of the stream. This can happen when reading from a readable stream, writing to a writable stream, or during any other stream operation.

```javascript
const { createReadStream } = require('fs');

let stream = createReadStream('/path/to/file');

stream.on('data', (chunk) => {
    console.log(`Received ${chunk.length} bytes of data.`);
});

stream.on('error', (error) => {
    console.error('An error occurred:', error);
});
```

```css
An error occurred: [Error: ENOENT: no such file or directory, open '/path/to/file'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: '/path/to/file'
}
```
 
 You can use this callback to handle the error appropriately, such as logging it or taking other corrective actions.

### HTTP server reading large file
This Node.js code creates an HTTP server that reads a large text file (`big.txt`) and streams its content to the client.
```js
const http = require('http')
const fs = require('fs')

let server = http.createServer((req, res) => {
    // const text = fs.readFileSync('/home/abishai/Documents/node/content/big.txt', 'utf-8')
    // res.end(text)

    let stream = fs.createReadStream('/home/abishai/Documents/node/content/big.txt', 'utf-8');
    stream.on('open', () => {
        stream.pipe(res);
    });
    stream.on('error', (err) => {
        res.end('Internal Server Error');
    });
})

server.listen(2000, () => {
    console.log("Listening on port 2000");
})
```
In this code, `fs.createReadStream()` creates a readable stream for the file `big.txt`, and `stream.pipe(res)` pipes the contents of the stream to the response `res`. This way, the file is sent to the client in smaller chunks, allowing you to open the header tab in the browser's developer tools.

You can use `res.end()` with a readable stream in Node.js. When you pipe a readable stream to the response object (`res`), you typically don't need to manually call `res.end()` because the stream will automatically end when it has been fully consumed.