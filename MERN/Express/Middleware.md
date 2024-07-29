Middleware functions in Express.js are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application's request-response cycle. These functions can perform tasks such as:

1. **Executing code**: Middleware functions can execute any code, make changes to the request and response objects, or call other middleware functions.

2. **Modifying the request and response objects**: Middleware functions can modify the request and response objects by adding properties, modifying existing properties, or attaching new data to them.

3. **Ending the request-response cycle**: Middleware functions can end the request-response cycle by sending a response back to the client. If a middleware function doesn't end the request-response cycle, it must call `next()` to pass control to the next middleware function or route handler.

4. **Calling the next middleware function in the stack**: If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. This allows for chaining multiple middleware functions together.

5. **Handling errors**: Middleware functions can handle errors that occur in the application by passing the error to the next middleware function using `next(error)`.

Middleware functions are executed in the order they are defined in the application. They can be defined at various levels:

1. **Application-level middleware**: These middleware functions are executed for every request made to the application. They are defined using `app.use()`.

2. **Router-level middleware**: These middleware functions are executed for requests made to specific routes or route groups. They are defined using `router.use()`.

3. **Error-handling middleware**: These middleware functions are specifically designed to handle errors that occur in the application. They are defined with four parameters (`err`, `req`, `res`, `next`).

4. **Built-in middleware**: Express.js comes with a set of built-in middleware functions for common tasks, such as serving static files (`express.static`), parsing request bodies (`express.json` and `express.urlencoded`), and handling HTTP method overrides (`method-override`).

5. **Third-party middleware**: Express.js also supports the use of third-party middleware functions, which can be installed via npm and used to add functionality to the application.

Middleware functions provide a way to modularize and organize your application's functionality, making it easier to manage and maintain. They allow you to perform common tasks, such as logging, authentication, error handling, and more, in a centralized and reusable manner.

```js
const express = require('express')
const app = express()
const { products } = require('./data')

const logger = (req, res, next) => {
    console.log(req.method, req.url, new Date().getFullYear());
    next()
}

app.get('/', logger, (req, res) => {
    res.send('Main Page')
})
app.get('/about', logger, (req, res) => {
    res.send('About Page')
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

In the provided example, `logger` is a middleware function that logs the request method, URL, and the current year to the console whenever a route is accessed. Middleware functions in Express.js have access to the request object (`req`), the response object (`res`), and the `next` middleware function in the application's request-response cycle.

Here's how the middleware `logger` works:

1. The `logger` function is defined with three parameters: `req` (the request object), `res` (the response object), and `next` (the next middleware function in the stack).
2. Inside the `logger` function, the `console.log` statement logs the request method (`req.method`), the requested URL (`req.url`), and the current year (`new Date().getFullYear()`).
3. After logging the necessary information, the `next()` function is called. This passes control to the next middleware function in the stack or, if no other middleware functions exist, to the route handler.

In the example, the `logger` middleware is applied to two routes: the root route (`/`) and the `/about` route. When a client makes a request to either of these routes, the `logger` middleware will log the request details to the console before passing control to the respective route handler.

They can be applied globally (for all routes) or selectively (for specific routes). Middleware functions are executed in the order they are defined, and you can have multiple middleware functions chained together to perform different tasks.

#### app.use()
![[Pasted image 20240429211620.png]]

```js
// logger.js
const logger = (req, res, next) => {
    console.log(req.method, req.url, new Date().getFullYear());
    next()
}

module.exports = logger
```

```js
const express = require('express')
const app = express()
const logger = require('./logger')

app.use('/about', logger)

app.get('/', (req, res) => {
    res.send('Main Page')
})
app.get('/about', (req, res) => {
    res.send('About Page')
})
app.get('/about/api', (req, res) => {
    res.send('About Page')
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

`app.use()` method is used in Express.js to mount middleware functions. Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application's request-response cycle. These functions can perform tasks such as logging, parsing request bodies, adding response headers, and more.

In the code you provided, `app.use('/about', logger)` means that the `logger` middleware function will be executed for all HTTP requests that start with the `/about` path. For example, requests to `/about`, `/about/api`, `/about/something-else`, etc. will all go through the `logger` middleware.

##### Multiple Middleware
```js
app.use([logger, authorize])
```

#### 3rd Party Middleware
```js
const express = require('express')
const app = express()
const morgan = require('morgan')

// Use only 'combined' morgan configuration
app.use(morgan('combined'))

app.get('/', (req, res) => {
    res.send('Main Page')
})
app.get('/about', (req, res) => {
    res.send('About Page')
})
app.get('/about/api', (req, res) => {
    res.send('About API Page')
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```
