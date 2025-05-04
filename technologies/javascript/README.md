# Questions for JavaScript

### 01. How do you detect primitive or non-primitive value types in JavaScript?
In JavaScript, values are generally categorized as either primitive or non-primitive (also known as reference types). Primitive values include:
  - Number: Represents numeric values.
  - String: Represents textual data.
  - Boolean: Represents true or false.
  - Undefined: Represents an uninitialized variables or absence of a value.
  - Null: Represents the intentional absence of any object value.
  - Symbol: Represents a unique identifier.

Non-primitive values are objects, which include arrays, functions, and custom objects. We can detect primitive or non-primitive in JavaScript in the following ways:

Using the 'typeof' operator 
```javascript
  // Using the typeof operator:
  let num = 10;
  let str = "Hello";
  let bool = true;
  let obj = {};
  let func = function() {};
  
  console.log(typeof num); // Output: "number"
  console.log(typeof str); // Output: "string"
  console.log(typeof bool); // Output: "boolean"
  console.log(typeof obj); // Output: "object"
  console.log(typeof func); // Output: "function"
```
Important: typeof null returns 'object' even though its a primitive value.

Using the Object() constructor:
```javascript
  // Using the Object() constructor:
  console.log(num === Object(num)); // Output: true
  // (primitive)
  console.log(obj === Object(obj)); // Output: false
  // (non-primitive)
```

### 02. Explain the key features introduced in JavaScript ES6.
In ES6, JavaScript introduced these key features:
- Arrow Functions: Consice syntax for anonymous functions with lexical scoping.
- Template Literals: Enables multiline strings and variables inclusion for improved readability.
- Destructuring Assignment: Simplifies extraction of values from arrays or objects.
- Enhanced Object Literals: Introduces shorthand notation for defining object methods and dynamic property names.
- Promises: Streamlines asynchronous programming with a cleaner structured approach.

### 03. What are the differences between var, let & const in JavaScript?
- Scope: 
  - Var is Functional scope, 
  - Let & Const are block scope.
- Update/Re-declaration:
  - Var, can be updated and re-declared within the scope.
  - Let can be updated but cannot be re-declared within the scope.
  - Const, cannot be updated or re-declared within the scope.
- Declaration without initialization:
  - Var, can be declared without being initialized.
  - Let, can be declared without being initialized.
  - Const, cannot be declared without being initialized.
- Access without Initialization:
  - Var, Accessible without initialization (default: undefined)
  - Let, Inaccessible without initialization (throws 'ReferenceError')
  - Const, Inaccessible without initialization (throws 'ReferenceError')
- Hoisting:
  - Var, Hoisted and initialized with a 'default' value.
  - Let, Hoisted but not initialized (error if accessed before declaration/initializaton).
  - Const, Hoisted but not initialized (error if accessed before declaration/initializaton).

### 04. What are arrow functions in JavaScript?
Arrow functions are a concise way to write anonymous function expressions in JavaScript. They were introduced in ECMAScript 6 (ES6) and are specially usefull for short, single-expression functions.
```javascript
  const add = (a, b) => a + b;
```

Here is an example showing how both traditional function expression and arrow function to illustrate the difference in handle the this keyword.
```javascript
  // Define an object
  let obj1 = {
    value: 42,
    valueOfThis: function() {
      return this.value; // 'this' refers to the object calling the function (obj1)
    }
  };

  // Call the method
  console.log(obj1.valueOfThis()); // Output: 42
```

In this example, obj1.valueOfThis() returns the value property of obj1, as this inside the function refers to the object obj1.

Arrow Function:
```javascript
  // Define an object
  let obj2 = {
    value: 84,
    valueOfThis: () => {
      return this.value; // 'this' does not refers to obj2; It inherits from the parent scope (windows in this case)
    }
  };

  // Call the method
  console.log(obj2.valueOfThis()); // Output: undefined or an error (depending on the environment)
```

In the arrow function within obj2, this does not refer to ojb2. INstead, it inherits its value from the parent scope, which is the global object (window in a browser environment). Consequently, obj2.valueOfThis() returns undefined or may even throw an error, as this.value is not defined in the global scope.

### 05. What is hoisting in JavaScript?
In JavaScript, hoisting is a phenomenon where variable and function declarations are conceptuallymoved to the top of their respective scopes, even if they're written later in the code. This behaviour applies to both global and local scopes.

```javascript
  // Variable hoisting
  console.log(myMessage); // Outputs 'undefined', not an error.
  var myMessage = 'Greetings!';
```

While myMessage appears declared after its use, it's hoisted to the top of the scope, allowing its reference (but not its initial value) before the actual declaration line.

```javascript
  // Function hoisting
  sayHello(); // Outputs "Hello, world!"

  function sayHello() {
    console.log('Hello, World!');
  }
```

Even though sayHello is defined after its call, JavaScript acts as if it were declared at the beginning of the scope, enabling its execution.

```javascript
  // Hoisting within Local Scopes
  function performTask() {
    result = 100; // Hoisted within the function
    console.log(result); // Outputs 100
    var result;
  }
  performTask();
```

Hoisting also occurs within local scopes, like functions. Here, result is hoisted to the top of the performTask function, allowing its use before its explicit declaration.

### 06. What is Strict Mode in JavaScript?
Strict Mode is a feature that allows you to place a program, or a function, in a 'strict' operating context. This way it prevents certain actions from being taken and throws more exceptions. The literal expression 'use strict' instructs the browser to use the JavaScript code in the Strict Mode. Strict Mode, helps to writing 'secure' JavaScript by notifying 'bad syntax' into real errors.

```javascript
  'use strict';
  x = 15; // ReferenceError: x in not defined.

  function strict_function() {
    'use strict';
    x = 'Teste message';
    console.log(x)
  }
  strict_function(); // ReferenceError: x is not defined.
```

### 07. What is NaN in JavaScript?
The NaN property in JavaScript represents a value that is 'Not-a-Number', indicating an illegal or undefined numeric value. When checking the type of NaN using 'typeof' operator, it returns "Number". To determine if a value is NaN, the isNaN() function is employed. It converts the given value to a Number type and then checks if it equals NaN.

```javascript
  // Examples
  isNaN("Hello"); // Returns true, as "Hello" cannot be converted to a valid number
  isNaN(NaN); // Returns true, as NaN is, by definition, Not-a Number
  isNaN("123ABC"); // Returns true, as "123ABC" cannot be converted to a valid number
  isNaN(undefined); // Returns true, as undefined cannot be converted to a valid number
  isNaN(456); // Returns false, as 456 is a valid numeric value
  isNaN(true); // Returns false, as true is converted to 1, a valid number
  isNaN(false); // Returns false, as false is converted to 0, a valid number
  isNaN(null); // Returns false, as null is converted to 0, a valid number  
```

### 08. Is JavaScript a statically typed or a dynamically typed language?
JavaScript is a dynamically typed language. In a dinamically typed language, variables types are determined at runtime, allowing a variable to hold values of any type without explicit type declarations.

### 09. What is HOFs (High Order Funcitons)?
Functions that treat other functions as values, either by:
- Taking one or more functions as arguments;
- Returning a function as a result;

Common examples of Built-in HOFs:
- map():
  - Applies a function to each element of an array and creates a new array with the results.  
  ```javascript
  // Examples
  const numbers = [1, 2, 3, 4, 5];
  const doubleNumbers = numbers.map(number => number * 2); // [2, 4, 6, 8, 10];
  ```
- filter():
  - Creates a new array containing only elements that pass a test implemented by a provided function.
  ```javascript
  // Examples
  const numbers = [1, 2, 3, 4, 5];
  const evenNumbers = numbers.filter(number => number % 2 === 0); // [2, 4];
  ```
- reduce():
  - Applies a function against an accumulator and each element in an array (from left to right) to reduce it to a single value.
  ```javascript
  // Examples
  const numbers = [1, 2, 3, 4];
  const sumOfNumbers = numbers.reduce((acc, num) => acc + num, 0); // 10;
  ```
- Creating Custom HOFs:
  - You can define your own HOFs to encapsulate common patterns and operations:
  ```javascript
  function createMultiplier(factor) {
    return number => number * factor;
  }

  const triple = createMultiplier(3);
  const tripledNumbers = numbers.map(triple); // [3, 6, 9, 12, 15]
  ```
  
### 10. What is difference between Null and Undefined?
- Null, is a absence of a value for a variable. Converted to zero (0).
- Undefined, is indicates the absence of the variable itself. Converted to NaN during primitive operations.

### 11. What is DOM?
DOM stands for Document Object Model, serving as a programming interface for web documents.
- Tree Structure: It represents the document as a tree, with the document object at the top and elements, attributes, and text forming the branches.
- Objects: Every document component (element, attribute, text) is an object in the DOM, allowing dynamic manipulation through programming languages like JavaScript.
- Dynamic Interaction: Enables real-time updates and interactions on web pages by modifying content and structure in response to user actions.
- Programming Interface: Provides a standardized way to interact with a web document, accessible and modifiable using scripts.
- Cross-plataform and Language-Agnostic: Not bound to a specific language and works across various web browsers, ensuring a consistent approach to document manipulation.
- Browser Implementation: While browsers have their own DOM implementations, they follow standars set by the World Wide Web Consortium (W3C), ensuring uniform in document representation and manipulation.

### 12. What is BOM?
BOM (Browser Object Model) is a programming interface extending beyong DOM, providing control over browser-related features.
- Window Object: Core BOM element representing the browser window, with properties and methods for browser control.
- Navigator, Location, History, Screen Objects: Components handling browser information, URL navigation, session history, and screen details.
- Document Object: Accessible throught BOM, allowing interaction with the structure of web pages.
- Timers: Functions like setTimeout and setInterval for scheduling code execution.
- Client Object: Represents user device information, aiding in responsive web design.
- Event object: Manages events triggered by user actions or browser events.