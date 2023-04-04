# Object & Function

## Object properties

* property creation

  ```javascript
  {
      const literal = { foo: 'bar' }
      console.log(literal)
  
      const byAssigning = {}
      byAssigning.foo = 'bar'
      console.log(byAssigning)
      
      function ConstructorFunction() {
          this.foo = 'bar'
      }
      const inConstructor = new ConstructorFunction()
      console.log(inConstructor)
  
      class Class {
          foo = 'bar'
  
          constructor() {
              this.foo2 = 'bar2'
          }
      }
      const classProperty = new Class()
      console.log(classProperty)
  
      const objectCreate = Object.create(Object.prototype, {
          foo: {
              value: 'bar'
          }
      })
      console.log(objectCreate)
  
      const objectDefineProperty = {}
      // also Object.defineProperties()
      Object.defineProperty(objectDefineProperty, 'foo', {
          value: 'bar'
      })
      console.log(objectDefineProperty)
  }
  ```
  
* operations on properties
  ```javascript
  const obj = {}
  
  // creation
  obj.foo = 'bar'
  
  // read
  console.log(obj.foo)
  
  // write
  obj.foo = 'baz'
  
  // enumerate
  for (const propertyName in obj) {
      console('property name of "obj"', propertyName)
  }
  
  // change properties
  Object.defineProperty(obj, 'foo', {
      value: 'bazbaz',
      enumerable: false,
      writable: false,
      configurable: true
  })
  
  // delete
  delete obj.foo
  ```
  
* properties of properties

  ```javascript
  globalThis.obj = {}
  
  // same as obj.foo = 'bar'
  Object.defineProperty(obj, 'foo', {
      value: 'bar',
      enumerable: true,
      writable: true,
      configurable: true
  })
  
  // defaults for creation vs. modification
  Object.defineProperty(obj, 'bar', {
      // value: undefined,
      // enumerable: false,
      // writable: false,
      // configurable: false
  })
  
  // getters && setters
  Object.defineProperty(obj, 'gsFoo', {
      get: function () {
          return this.fooStorage
      },
      set: function (newValue) {
          this.fooStorage = newValue
      },
      enumerable: true,
      configurable: true
  })
  
  // not enumerable
  Object.defineProperty(obj, 'notEnumerable', {
      value: 'bar',
      enumerable: false,
      writable: true,
      configurable: true
  })
  
  
  // not writable
  Object.defineProperty(obj, 'notWritable', {
      value: 'bar',
      enumerable: true,
      writable: false,
      configurable: true
  })
  
  /*
   not Configurable, It's forbiddeden to
     * change convert between value and getter & setter
     * delete the property
     * change properties except for `writable` to false
  */
  Object.defineProperty(obj, 'notConfigurable', {
      value: 'bar',
      enumerable: true,
      writable: true,
      configurable: false
  })
  ```

  * sealed
  * frozen
  * Object.preventExtensions()

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