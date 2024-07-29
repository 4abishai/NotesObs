```java
const http = require('http')

// const server = http.createServer((req, res) => {
//     console.log("Server Hit");
//     res.end("Home Page", 'utf-8', () => {
//         console.log("User Hit home page");
//     });
// })

const server = http.createServer((req, res) => {
    res.writeHead(200, {
        'content-type': 'text/html'
    })
        .write('<h1>Home Page</h1>', () => {
            console.log("User hit the home page");
        })
    res.end();
})

server.listen(5000)
```

#### Status Codes
Status codes are standardized numerical codes that a server sends in response to a client's request made to a web server. These codes indicate the outcome of the request and help both the client and the server understand how to proceed. Status codes are divided into five categories, each represented by the first digit of the status code:

- **1xx (Informational):** These status codes indicate that the server has received the request and is continuing the process.
- **2xx (Success):** These status codes indicate that the request was received, understood, and processed successfully.
- **3xx (Redirection):** These status codes indicate that further action needs to be taken by the client to complete the request.
- **4xx (Client Error):** These status codes indicate that the client has made an error in the request, such as requesting a page that doesn't exist.
- **5xx (Server Error):** These status codes indicate that the server encountered an error while trying to fulfill the request.

Some common status codes include:

- **200 OK:** The request was successful.
- **404 Not Found:** The requested resource could not be found on the server.
- **500 Internal Server Error:** A generic error message indicating a problem on the server.

Status codes are an essential part of the HTTP protocol, providing a way for servers and clients to communicate about the status of requests and responses.
#### MIME Types
MIME (Multipurpose Internet Mail Extensions) types are a standard way of indicating the type of content that is being sent in a message over the Internet. They are used in various contexts, such as email attachments and HTTP responses, to specify the format of the data being transmitted. MIME types consist of a type and a subtype, separated by a slash (/). 

For example, in the MIME type "text/html", "text" is the type and "html" is the subtype, indicating that the content is HTML formatted text. Some common MIME types include:

- text/plain: Plain text
- text/html: HTML document
- application/json: JSON data
- image/jpeg: JPEG image
- audio/mpeg: MPEG audio file
- video/mp4: MP4 video file

MIME types are important for web servers to correctly interpret and handle different types of files being requested by clients, such as web browsers. They help ensure that the content is delivered and rendered properly based on its format.

### HTTP app
```js
const http = require('http')
const { readFileSync } = require('fs')

//get all the files
const home = readFileSync('/home/abishai/Desktop/node-express-course/02-express-tutorial/navbar-app/index.html')
const icon = readFileSync('/home/abishai/Desktop/node-express-course/02-express-tutorial/navbar-app/logo.svg')
const style = readFileSync('/home/abishai/Desktop/node-express-course/02-express-tutorial/navbar-app/styles.css')
const logic = readFileSync('/home/abishai/Desktop/node-express-course/02-express-tutorial/navbar-app/browser-app.js')

const server = http.createServer((req, res) => {
    let url = req.url
    console.log(url);
    if (url == '/') {
        res.writeHead(200, { 'content-type': 'text/html' })
        res.write(home)
        res.end()
    }
    else if (url == '/styles.css') {
        res.writeHead(200, { 'content-type': 'text/css' })
        res.write(style)
        res.end()
    }
    else if (url == '/logo.svg') {
        res.writeHead(200, { 'content-type': 'image/svg+xml' })
        res.write(icon)
        res.end()
    }
    else if (url == '/browser-app.js') {
        res.writeHead(200, { 'content-type': 'text/javascript' })
        res.write(logic)
        res.end()
    }
    else {
        res.writeHead(404, { 'content-type': 'text/html' })
        res.write('<h1>Page Not Found</h1>')
        res.end()
    }
})

server.listen(5000, ()=> {
	console.log('Server listening on port: 5000')
})
```