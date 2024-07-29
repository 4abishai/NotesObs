In Node.js, the equivalent of Python's `os` module is the `os` module. It provides a way to interact with the operating system on which Node.js is running. Here's a simple showcase of how you can use the `os` module in Node.js:

```javascript
// Import the os module
const os = require('os');

// Get the current user's home directory
const homeDirectory = os.homedir();
console.log('Home directory:', homeDirectory);

// Get the hostname of the machine
const hostname = os.hostname();
console.log('Hostname:', hostname);

// Get information about the operating system platform
const platform = os.platform();
console.log('Platform:', platform);

// Get information about the operating system architecture
const arch = os.arch();
console.log('Architecture:', arch);

// Get information about the number of CPU cores
const cpuCores = os.cpus().length;
console.log('Number of CPU cores:', cpuCores);

// Get the total system memory
const totalMemory = os.totalmem();
console.log('Total memory:', totalMemory / (1024 * 1024 * 1024), 'GB');

// Get the free system memory
const freeMemory = os.freemem();
console.log('Free memory:', freeMemory / (1024 * 1024 * 1024), 'GB');

// Get information about the network interfaces
const networkInterfaces = os.networkInterfaces();
console.log('Network Interfaces:', networkInterfaces);
```

This code showcases various functionalities of the `os` module such as retrieving the current user's home directory, hostname, platform, architecture, CPU core count, total and free system memory, and information about network interfaces.

Remember to run this code in a Node.js environment to see the output.