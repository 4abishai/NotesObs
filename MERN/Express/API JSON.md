APIs (Application Programming Interfaces) are interfaces that allow different software applications to communicate with each other over the HTTP protocol, among other protocols. HTTP APIs typically use REST (Representational State Transfer) principles to define how resources are accessed and manipulated using standard HTTP methods like GET, POST, PUT, DELETE, etc. These interfaces define the rules and protocols for interacting with a particular software application or service, enabling developers to access its functionality and data.

#### Basic JSON response
```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.json([{ 'name': 'Abishai' }, { 'name': 'Mark' }])
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

#### Serving a JSON response from a file
```js
const express = require('express')
const app = express()
const { products } = require('./data')


app.get('/', (req, res) => {
    res.json(products)
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```
`products` is an array of objects that represent products

#### Serving specific properties
```js
const express = require('express')
const app = express()
const { products } = require('./data')


app.get('/', (req, res) => {
    const newProducts = products.map((p) => {
        const { id, name, image } = p
        return { id, name, image }
    })
    res.json(newProducts)
})

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```
For each product `p` in the `products` array, a new object is created containing only the `id`, `name`, and `image` properties from the original product object `p`.
##### Alternative
```js
const express = require('express');
const app = express();
const { products } = require('./data');

app.get('/', (req, res) => {
    const newProducts = products.map(({ id, name, image }) => ({ id, name, image }));
    res.json(newProducts);
});

app.listen(5000, () => {
    console.log('Server is listening on port: 5000');
});
```
The transformation function in this case is an arrow function that uses object destructuring to extract the `id`, `name`, and `image` properties from each product object. It then immediately returns a new object literal `{ id, name, image }` using the extracted properties.
