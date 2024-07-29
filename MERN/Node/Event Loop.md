JS reads everything line by line
![[Pasted image 20240331161701.png]]

Browser provides APIs so that time consuming codes are offloaded. Here `second task` is offloaded.
![[Pasted image 20240331161751.png]]
![[Pasted image 20240331162203.png]]Event loop registers the callback and only executes the callback once the task is complete

## Offloading readFile

Event loop will offload async `readFile` method to a file system. Only when the reading is completed (whether its a response or an error) is the callback executed.
![[Pasted image 20240331163140.png]]

Only when the synchronous code is executed the async code is executed
![[Pasted image 20240331163421.png]]
As setTImeout is asynchronous the callback is executed after all the sync codes are done executing.

### Blocking Code
```js
const { createServer } = require('http')

const server = createServer((req, res) => {
    if (req.url === "/") {
        res.end("Home Page")
    } else if (req.url === "/about") {
        // BLOCKING CODE!!!
        for (let i = 0; i < 1000; i++) {
            for (let j = 0; j < 1000; j++) {
                console.log(`${i} ${j}`);
            }
        }
        res.end("About Page")
    } else {
        res.end("Error: Page not found")
    }
    console.log("Request Event")
})

server.listen(5000, () => {
    console.log("Server listening on port 5000");
})
```

Because of asynchronous blocking code the execution of latter code is halted. That's why we need asynchronous code.

### Promise

```js
const { readFile, writeFile } = require('fs')

console.log('start')
// getData returns Promise object
const getData = (path) => {
    return new Promise((resolve, reject) => {
        readFile(path, 'utf8', (err, res) => {
            if (err) {
                reject(err)
            }
            resolve(res)
        })
    })
}

getData("/home/abishai/Documents/node/content/a.txt")
    .then((res) => {
        console.log(res);
    })
    .catch((err) => {
        console.log(err);
    })

console.log('starting next task')
```

```css
start
starting next task
Hello this is file a.
```

### Async Await
If we want to read from multiple file reads or preform file read and write the code will still be messy as we will have to settle for nested callbacks. So we use async-await.


```js
const start = async () => {
    try {
        let file1 = await getData("/home/abishai/Documents/node/content/a.txt")
        console.log(file1);
    } catch (err) {
        console.log(err);
    }
}

start()
```

```js
const start = async () => {
    try {
        let file1 = await getData("/home/abishai/Documents/node/content/a.txt")
        let file2 = await getData("/home/abishai/Documents/node/content/b.txt")
        console.log(file1);
        console.log(file2);
    } catch (err) {
        console.log(err);
    }
}

start()
```

```css
start
starting next task
Hello this is file a.
Hello this is file b.
```

### Util Module Promisify
