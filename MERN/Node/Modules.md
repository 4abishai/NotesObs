Sure, here are some example programs demonstrating the usage of different types of modules in Node.js:

### 1. **Core Module Example** (Using the `fs` module to read a file):

```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) {
        console.error('Error reading file:', err);
        return;
    }
    console.log('File content:', data);
});
```

### 2. **Local Module Example** (Creating and using a local module to perform some operations):

1. **Variable Declarations**: 
    - `secret`: A constant variable holding the string "Secret Val".
    - `share1` and `share2`: Two constant variables holding strings "Abishai" and "Ekka" respectively.

2. **Exporting Variables**: 
    ```javascript
    module.exports = { share1, share2 }
    ```
    This line exports the `share1` and `share2` variables from this module, making them available for use in other modules.

3. **Function Definition**: 
    ```javascript
    let printString = (s) => {
        console.log("hello ", s)
    }
    ```
    This defines a function `printString` which takes a parameter `s` and logs "hello" followed by the value of `s`.

4. **Reassigning `module.exports`**: 
    ```javascript
    module.exports = printString
    ```
    This line reassigns the `module.exports` to the `printString` function. Therefore, now this module exports only the `printString` function, replacing the previous export of `{ share1, share2 }`.

5. **Importing Modules**: 
    ```javascript
    const imported = require('./module3')
    const printString = require('./func')
    ```
    - `imported`: Imports the module exported from `'./module3'`.
    - `printString`: Imports the module exported from `'./func'`.

6. **Console Outputs**:
    ```javascript
    console.log(imported)
    ```
    This logs the entire `imported` module.

    ```javascript
    console.log(imported.share1)
    console.log(imported.share2)
    ```
    These lines log the values of `share1` and `share2` from the `imported` module.

    ```javascript
    printString(imported.share1)
    printString(imported.share2)
    ```
    These lines call the `printString` function with `imported.share1` and `imported.share2` as arguments, which prints "hello" followed by the respective values.

### 3. **Third-party Module Example** (Using the `axios` module to make an HTTP request):

```javascript
const axios = require('axios');

axios.get('https://api.example.com/data')
    .then(response => {
        console.log('Data:', response.data);
    })
    .catch(error => {
        console.error('Error fetching data:', error);
    });
```

### 4. **ES6 Module Example** (Using ECMAScript Modules syntax):

First, let's define the `module3.js` file:

```javascript
// module3.js
export const share1 = "Abishai";
export const share2 = "Ekka";
```

Now, let's define the `func.js` file:

```javascript
// func.js
const printString = (s) => {
    console.log("hello ", s);
}

export default printString;
```

Finally, let's define the main file `main.js`:

```javascript
// main.js
import { share1, share2 } from './module3';
import printString from './func';

console.log(share1);
console.log(share2);

printString(share1);
printString(share2);
```

In this version, we use `export` statements to export the variables and functions in the respective files. Then, in the `main.js` file, we use `import` statements to import these variables and functions as needed. This is the syntax for ES6 modules, which is a standardized module system in JavaScript.

### Notes

### 1
When you import a module that contains executable code (such as function calls or variable declarations at the root level), that code will be executed immediately upon importing the module, even if you haven't assigned it to a variable.

For example, consider the following module named `module.js`:

```javascript
// module.js
console.log("Module has been imported!");

const myFunction = () => {
    console.log("Executing myFunction!");
}

myFunction();
```

Now, if you import this module in another file, say `main.js`, like so:

```javascript
// main.js
import './module.js';

console.log("Main module has been executed!");
```

When you run `main.js`, you'll see the following output:

```
Module has been imported!
Executing myFunction!
Main module has been executed!
```

As you can see, even though `myFunction` wasn't assigned to a variable, it still executed upon importing `module.js`. This behavior is consistent with how JavaScript modules work—any code at the root level of the module will run upon import.

