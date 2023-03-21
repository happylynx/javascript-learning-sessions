# Object & Function

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
    * default arguments
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
        * local functions

          ```javascript
          function a() {
              console.log(a)
              b()
              console.log(a, window.a, b, window.b)
              function b() {
                  console.log('b')
              }
          }
          a()
          b()  // it fails
          ```

          ```javascript
          function a() {
              console.log('a')
              b()
              let b = 3
              function b() { // SyntaxError: redeclaration of let b
                  console.log
              }
          }
          ```
    * lambdas
        * `this` not bound
    * immediately invoked function