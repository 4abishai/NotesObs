#### Specific routes for products
```js
const express = require('express')
const app = express()
const { products } = require('./data')

app.get('/', (req, res) => {
    res.send('<a href="/api/products/">PRODUCTS</a>')
})

app.get('/api/products', (req, res) => {
    const newProducts = products.map(({ id, name, image }) => ({ id, name, image }));
    res.json(newProducts)
})

app.get('/api/products/1', (req, res) => {
    const newProduct = products.find((p) => p.id === 1);
    res.json(newProduct);
});

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

Route parameters in Express allow you to create flexible routes that can handle varying inputs, such as product IDs, user IDs, or any other **dynamic data** in the URL path.

```js
const express = require('express')
const app = express()
const { products } = require('./data')

app.get('/', (req, res) => {
    res.send('<a href="/api/products/">PRODUCTS</a>')
})

app.get('/api/products', (req, res) => {
    const newProducts = products.map(({ id, name, image }) => ({ id, name, image }));
    res.json(newProducts)
})

app.get('/api/products/:pid', (req, res) => {
    console.log(req.params)
    const { pid } = req.params

    const singleProduct = products.find((p) => p.id === parseInt(pid))
    if (singleProduct)
        res.json(singleProduct);
    else
        res.status(404).send('Product does not exist')
});

app.listen(5000, () => {
    console.log('Server is listening on port: 5000')
})
```

The route parameter `:pid` in the route definition `/api/products/:pid` allows you to create a dynamic route that can handle requests for different product IDs.

When a request is made to a route like `/api/products/1`, Express extracts the value `1` from the URL and makes it available in the `req.params` object under the key `pid`. This allows you to use the value `1` (or any other value in place of `1`) to identify the specific product requested by the client.

