### Handling Form Submission
##### Request: Content-type: application/urlencoded
```js
const express = require('express')
const app = express()

// staic assets
app.use(express.static('./methods-public'))
// parse form data
app.use(express.urlencoded({ extended: false }))

app.post('/login', (req, res) => {
    const { name } = req.body
    if (name)
        res.status(200).send(`Hello ${name}`)
    else
        res.status(401).send("Name cannot be blank")
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

The line `app.use(express.urlencoded({ extended: false }))` sets up a middleware function in the Express.js application to parse incoming request bodies in URL-encoded format.

In web applications, when you submit a form or send data using certain types of HTTP requests (like POST or PUT), the data is often encoded in the request body. One common encoding format is URL-encoded, where the data is represented as key-value pairs separated by the equal sign (`=`), and pairs are separated by the ampersand (`&`). For example, `name=John&age=30`.

The `express.urlencoded` middleware is responsible for parsing this URL-encoded data and making it available in the `req.body` object of the incoming request.

The `{ extended: false }` option tells the middleware to use the classic URL-encoded parser, which is a more minimalistic parser. Setting `extended` to `true` would use the `qs` library for parsing, which allows more complex data types and nested objects.

In most cases, using `extended: false` is sufficient for parsing simple URL-encoded data. However, if you need to parse more complex data structures or nested objects, you might need to set `extended: true`.

After setting up this middleware, when the server receives a request with URL-encoded data in the request body, the middleware will parse the data and make it available in `req.body`. For example, if the request body contains `name=John&age=30`, you can access these values using `req.body.name` and `req.body.age` in your route handlers.

It's important to note that this middleware only parses URL-encoded data in the request body. If you need to parse other data formats, like JSON or raw data, you would need to use different middleware functions provided by Express.js or third-party libraries.

### Handling JSON data
##### Request: Content-type: application/json

```js
const express = require('express')
const app = express()
const { people } = require('./data')

// staic assets
app.use(express.static('./methods-public'))
// parse json
app.use(express.json())

// GET method
app.get('/api/people', (req, res) => {
    res.status(200).json({ success: true, data: people })
})

// POST method (server checks whether a name is provided)
app.post('/api/people', (req, res) => {
    let { name } = req.body
    if (name)
        res.status(200).json({ success: true, person: name })
    else
        res.status(401).json({ success: false, msg: 'Please provide a name' })
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

`app.use(express.json())` enables parsing of JSON data in the request body.

The code provides two routes:

1. `GET /api/people`: This route returns the `people` data as a JSON response.
2. `POST /api/people`: This route expects a JSON payload in the request body with a `name` property. If the `name` is provided, it responds with a success status and the provided `name`. If the `name` is not provided, it responds with a 401 Unauthorized status and an error message.

The code demonstrates handling JSON data using Express.js. It parses the JSON request body using `app.use(express.json())` and sends JSON responses using `res.json()`.