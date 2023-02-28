* basics
* Object & Function
    * object properties
        * sealed
        * frozen
        * getters & setters
    * creation of object
        * literal
        * `Object.create`
        * `new`
        * `Object.fromEntries()`
        * btw. `Object.assign()`
    * object prototype
        * chain
        * own properties, symbols
    * `Object.prototype.toString()`
    * Function
        * named and anonymous
        * function, constructor, method
        * `this`
            * outside function
                * global object 
            * inside regular function
                * strict mode / modules
                * global object / `undefined`
            * inside method
                * object on which it is called
            * inside constructor
                * new object
                * `new.target === fn`
        * `Function.name`
        * `Function.prototype`
        * `Function.length`
        * `Object.prototype.constructor`
        * `instanceof` operator
            * `Symbol.hasInstance`
        * `bind`, `apply`, `call`
        * creation
            * function literal
            * method definition
            * `Function` constructor
        * lambdas
            * `this` not bound
            * `void` operator
        * immediately invoked function
* Array, collections, iterable protocol
    * `Array.isArray()`
    * array methods
        * eagerly evaluated
    * iterable protocol
        * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols
        * iterable
            * `Symbol.iterator`
        * iterator
            * `next(): { done=false, value? }`
            * `return(value): IteratorResult`
            * `throw(exception): IteratorResult`
    * `Array.form()`
    * `for ... in`
    * `for ... of`
    * Set, Map
    * WeakSet, WeakMap
    * "range" function
    * array empty slots
* Generator, generator function, async await, Promise
    * generator
    * generator function
        * `*` notation
        * `yield`
            * return value of yield expression
        * `yield*`
    * event loop
        * single thread processing
    * callbacks
    * `setTimeout()`
    * `Promise`
        * constructor
        * `.then()`
        * `.catch()`
        * `.finally()`
        * static methods
        * swallowed exception
    * async functions
        * `await`
            * exception handling
    * async generator function
    * `for await ... of`
* `...`, destructuring
    * destructuring
        * arrays, objects
        * arguments
        * assignment
    * vararg
* strict mode
    * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode
* Java-like classes
* modules
* Binary data
* Build-in objects
    * Object
    * Array
    * String
    * Regexp
    * Number
    * Math
    * Proxy & Reflect
    * JSON
    * Weak*