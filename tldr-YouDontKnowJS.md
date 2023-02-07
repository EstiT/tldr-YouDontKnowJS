# TLDR You Don't know JS - Book Notes


## Scope & Closures
**Scope** - accessibility / visibility of variables
**Lexical scoping** - scope is determined by where its located / the code block that its defined in, only accessible within that block and inner blocks
Function scoping - functions create a new scope
**Block scoping** - (let and const) variables declared outside of a block { } cannot be accessed
**Hoisting** - the JS interpreter brings the declaration of a variable to the top of the scope(hoists) vars are hoisted
**Closures** - inner functions that have access to outer functions variables and can maintain their values even after the outer function has returned 

## This and Object Prototypes
**`this`** - refers to the object the current function is a method of or the global object(window) or can be the element of an event listener, it refers to different objects depending on how it is used 
Object Prototypes - All JavaScript objects inherit properties and methods from a prototype(Array objects inherit from Array.prototype) Prototypes can be used to extend an Object - add new methods or properties.Allows for Inheritance 

```
function Person(first, last, age, eyeColor) {
        this.firstName = first;
        this.lastName = last;
        this.age = age;
        this.eyeColor = eyeColor;
    }

Person.prototype.nationality = "English";
```

**Inheritance** - allowing objects to reuse a class's methods and properties of a parent class and extend on them. Class inheritance uses the extends keyword 
```
class Model extends Car {
    constructor(brand, mod) {
        super(brand);
        this.model = mod;
    }
```

## Types and Coercion
**Type coercion** - JS automatically changes data types when some operations or comparisons are performed
```
let y = "5"; // string
y += y; // number 

‘5’ == 5 // true
```
Use === and explicitly convert values to desired data type(Number(x)) to avoid type coercion errors  

### Comparison and equality operators 
**Equality operators**:  == (performs type coercion)equal value, === (strict comparison) equal value and equal type 
**Relational operators**: >, <, >=, <= compare values and determine their relative order.Do not perform type coercion, only work on values of the same data type 


## Asynchronous and Performance
**Async / await** - makes asynchronous code look like synchronous code, makes code easier to understand.Async function can use await to wait for a promise to resolve
**Callbacks** - function that are passed as arguments to function and are executed when an event occurs
**Promises** - an object that may or may not be available yet, can access the value once ready 
**Event loop** - a continuous loop that waits for events and schedules the corresponding callback to be executed.Allows for JS to handle multiple events at the same time by adding them to a message queue and executing teh callbacks one by one.


## ES6
**Destructuring** - syntax used to extract values from arrays and objects into separate variables

```
let [first, second, third] = [1, 2, 3]; // Array destructuring

let person = { name: "John", age: 30 };
let { name, age } = person; // Object destructuring 
```
 
**Arrow functions** - easier to read and write syntax for writing functions, this is lexically scoped(inherited from the surrounding code) rather than determined dynamically like traditional functions.Not hoisted, no arguments object, no prototype property  
**Template literals** - strings that can include expressions to be evaluated dynamically, uses back ticks, can easily format strings with white space and line breaks
```
let x = ‘world’;
let s = `hello ${x}`; // hello world
```

**Modules** - way to organize code into reusable and isolated units that can be imported to be used by other parts of an application.Example below math.js is a module that exports two functions
```
// math.js
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}


// main.js
import { add, subtract } from "./math.js";

console.log(add(1, 2)); // Output: 3
console.log(subtract(2, 1)); // Output: 1
```

**Classes** - way to use OO features, provide a syntax for creating and extending objects along with methods and properties.Features include inheritance, static methods, getters, setters and more
```
class Person {
    constructor(name) {
        this.name = name;
    }

    sayHello() {
        console.log(`Hello, my name is ${this.name}.`);
    }
}

Generators - functions that can be paused and resumed multiple times.Allow for iterators to be created and controlled.Define them using function*
    Ex1 :
    function* generator() {
    yield 1;
    yield 2;
    yield 3;
}

let iterator = generator();
console.log(iterator.next().value); // Output: 1
console.log(iterator.next().value); // Output: 2
console.log(iterator.next().value); // Output: 3


Ex 2:
function* range(start, end) {
    while (start <= end) {
        yield start++;
    }
}

for (let value of range(1, 5)) {
    console.log(value);
}
// Output:
// 1
// 2
// 3
// 4
// 5

```

**Proxies** - an object that acts as a mediator between the original object and the code that it interacts with, can enhance and customize the interactions between objects and the code that uses them.Can be one of 13 different traps.Most commonly used are get, set, and apply

```
// Ex 1: object validation - set
const handler = {
    set: function (obj, prop, value) {
        if (typeof value !== "number") {
            throw new TypeError("Invalid value type, expected number");
        }
        if (value < 0 || value > 100) {
            throw new RangeError("Value out of range, expected 0-100");
        }
        obj[prop] = value;
        return true;
    }
};
const score = new Proxy({}, handler);
score.mark = 80;
console.log(score.mark); // 80
score.mark = "invalid"; // TypeError: Invalid value type, expected number
score.mark = 120; // RangeError: Value out of range, expected 0-100



// Ex 2: property access logging - get 
const handler = {
    get: function (obj, prop) {
        console.log(`Accessing property '${prop}' with value ${obj[prop]} at ${new Date()}`);
        return obj[prop];
    }
};
const user = { name: "John Doe", age: 30 };
const loggedUser = new Proxy(user, handler);
console.log(loggedUser.name); // Accessing property 'name' with value 'John Doe' at Mon Feb 06 2023 12:40:33 GMT-0500 (Eastern Standard Time)
// 'John Doe'
console.log(loggedUser.age);  // Accessing property 'age' with value 30 at Mon Feb 06 2023 12:40:33 GMT-0500 (Eastern Standard Time)
// 30



// Ex 3: method overriding - apply
const handler = {
    apply: function (target, thisArg, argumentsList) {
        console.log(`Calling method '${target.name}' with arguments [${argumentsList}]`);
        const result = target.apply(thisArg, argumentsList);
        console.log(`Method '${target.name}' returned ${result}`);
        return result;
    }
};
const multiply = (a, b) => a * b;
const loggedMultiply = new Proxy(multiply, handler);
console.log(loggedMultiply(3, 4)); // Calling method 'multiply' with arguments [3,4]
// Method 'multiply' returned 12
// 12

```
**Reflection** - the ability of an object to inspect and modify its own properties and behavior.
    
    
## Engine, Compiler and Runtime 
