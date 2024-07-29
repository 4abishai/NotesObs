A router in the context of a web application built with Express.js is used to define routes, which are essentially URLs or paths that the application will respond to when a client (such as a web browser) sends a request.
![[Pasted image 20240430171412.png]]

```js
const express = require('express')
const app = express()
const { people } = require('./data')

// staic assets
app.use(express.static('./methods-public'))
// parse json
app.use(express.json())

app.post('/login', (req, res) => {
    if (req.body.name)
        res.status(200).send(`Welcome ${req.body.name}`)
    else
        res.status(401).send('Please provide a name')
})

app.get('/api/people', (req, res) => {
    res.status(200).json({ success: true, data: people })
})

app.post('/api/people', (req, res) => {
    let { name } = req.body
    if (name)
        res.status(200).json({ success: true, person: name })
    else
        res.status(401).json({ success: false, msg: 'Please provide a name' })
})

app.put('/api/people/:id', (req, res) => {
    const { id } = req.params
    const { name } = req.body

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.map((p) => {
            if (p.id === parseInt(id))
                p.name = name
            return p
        })
        res.status(200).json({ success: true, data: newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})

app.delete('/api/people/:id', (req, res) => {
    const { id } = req.params

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.filter((p) => p.id !== person.id)
        res.status(200).json({ success: true, newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

Can be rewritten as.
```js
//app.js
const express = require('express')
const app = express()
const people = require('./routes/people')
const login = require('./routes/login')

// staic assets
app.use(express.static('./methods-public'))
// parse json
app.use(express.json())

app.use('/api/people', people)
app.use('/login', login)

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

```js
//login.js
const express = require('express')
const router = express.Router()

router.post('/', (req, res) => {
    if (req.body.name)
        res.status(200).send(`Welcome ${req.body.name}`)
    else
        res.status(401).send('Please provide a name')
})

module.exports = router
```

```js
//people.js
const express = require('express')
const router = express.Router()

const { people } = require('../data')

router.get('/', (req, res) => {
    res.status(200).json({ success: true, data: people })
})

router.post('/', (req, res) => {
    let { name } = req.body
    if (name)
        res.status(200).json({ success: true, person: name })
    else
        res.status(401).json({ success: false, msg: 'Please provide a name' })
})

router.put('/:id', (req, res) => {
    const { id } = req.params
    const { name } = req.body

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.map((p) => {
            if (p.id === parseInt(id))
                p.name = name
            return p
        })
        res.status(200).json({ success: true, data: newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})

router.delete('/:id', (req, res) => {
    const { id } = req.params

    const person = people.find((p) => p.id === parseInt(id))
    if (person) {
        const newPeople = people.filter((p) => p.id !== person.id)
        res.status(200).json({ success: true, newPeople })
    }
    else
        res.status(404).json({ success: false, msg: `no person with id ${id}` })
})

module.exports = router
```

In the provided code, there are two routers defined: `people` and `login`.

The `app.js` file is the entry point of the application. Here's what's happening in this file:

1. The Express module is imported, and an instance of the Express application is created using `express()`.
2. The `people` and `login` routers are imported from their respective files (`./routes/people` and `./routes/login`).
3. The `express.static` middleware is used to serve static files from the `./methods-public` directory.
4. The `express.json` middleware is used to parse incoming JSON data from requests.
5. The `/api/people` route is mounted using `app.use('/api/people', people)`, which means that any requests starting with `/api/people` will be handled by the `people` router.
6. The `/login` route is mounted using `app.use('/login', login)`, which means that any requests starting with `/login` will be handled by the `login` router.
7. The application starts listening on port 5000.

The `login.js` file defines the `login` router:

1. The Express module is imported, and a new router instance is created using `express.Router()`.
2. A POST route (`/`) is defined, which handles login requests.
   - If the request body contains a `name` property, it responds with a 200 status code and a welcome message.
   - If the request body does not contain a `name` property, it responds with a 401 status code and an error message.
3. The router is exported as a module using `module.exports = router`.

The `people.js` file defines the `people` router:

1. The Express module is imported, and a new router instance is created using `express.Router()`.
2. The `people` data is imported from `../data`.
3. A GET route (`/`) is defined, which responds with a 200 status code and the `people` data in JSON format.
4. A POST route (`/`) is defined, which handles requests to create a new person.
   - If the request body contains a `name` property, it responds with a 200 status code and the `name` in JSON format.
   - If the request body does not contain a `name` property, it responds with a 401 status code and an error message.
5. A PUT route (`/:id`) is defined, which handles requests to update a person's name.
   - The `id` is extracted from the request parameters (`req.params`).
   - The `name` is extracted from the request body (`req.body`).
   - If a person with the provided `id` exists, their name is updated, and the updated `people` data is returned with a 200 status code.
   - If no person with the provided `id` exists, a 404 status code and an error message are returned.
6. A DELETE route (`/:id`) is defined, which handles requests to delete a person.
   - The `id` is extracted from the request parameters (`req.params`).
   - If a person with the provided `id` exists, they are removed from the `people` data, and the updated `people` data is returned with a 200 status code.
   - If no person with the provided `id` exists, a 404 status code and an error message are returned.
7. The router is exported as a module using `module.exports = router`.

This code sets up an Express.js application with two main routes: `/api/people` and `/login`. The `/api/people` route handles CRUD operations (Create, Read, Update, Delete) for managing a collection of people, while the `/login` route handles a simple login functionality.