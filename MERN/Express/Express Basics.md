### Basic
```js
const express = require('express')
const app = express()
// const app = require('express')()

app.get('/', (req, res) => {
    console.log('User Hit')
    res.status(200).send('<h1>Home Page</h1>')
})

app.listen(5000, () => {
    console.log('Server listening on port: 5000')
})
```

![[Pasted image 20240428235841.png]]
