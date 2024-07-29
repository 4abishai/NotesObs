In JavaScript, destructuring allows you to extract multiple properties or elements from objects or arrays and assign them to variables in a single statement. You can destructure objects, arrays, and even modules in Node.js. Here's an example of destructuring a module in Node.js:

```javascript
// Importing a module
const os = require('os');

// Destructuring the module
const { hostname, platform, arch, cpus, totalmem, freemem } = os;

// Using the destructured properties
console.log('Hostname:', hostname());
console.log('Platform:', platform());
console.log('Architecture:', arch());
console.log('CPU Cores:', cpus().length);
console.log('Total Memory:', totalmem() / (1024 * 1024 * 1024), 'GB');
console.log('Free Memory:', freemem() / (1024 * 1024 * 1024), 'GB');
```

In this example, we're destructuring the `os` module directly when importing it. We're extracting specific properties and methods like `hostname`, `platform`, `arch`, `cpus`, `totalmem`, and `freemem` from the `os` module. This way, we can directly use these properties and methods without accessing them through the `os` object.

This technique can make your code cleaner and more concise, especially when you're only interested in a subset of the functionalities provided by a module.