# Object & Function

## Object properties

### Property creation

```javascript
{
    const literal = { foo: 'bar' }
    console.log(literal)

    const byAssigning = {}
    byAssigning.foo = 'bar'
    console.log(byAssigning)
    
    const gettersAndSetters = {
        get foo() {
            console.log('getter', this.value)
            return this.value
        },
        set foo(newValue) {
            console.log('setter', newValue)
            this.value = newValue
        }
    }
    console.log(gettersAndSetters)
    
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
  
### Operations on properties

```javascript
const obj = {}

// creation
obj.foo = 'bar'

// read
console.log(obj.foo)
console.log(obj['foo'])

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
  
### Properties of properties

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

console.log('obj', obj)

// Introspection
console.log('"nonWritable" property descriptor', Object.getOwnPropertyDescriptor(obj, 'notWritable'))
console.log('property descriptors', Object.getOwnPropertyDescriptors(obj))
```
  
<details>
<summary>Task</summary>

Create an object with property `foo` that


* can only be assigned integers. Assignments of other values are ignored.
* can only be read if value % 3 is 0. When value % 3 is 1 it is always read as `"1"`. When
value % 3 is 2, value can't be read - there is no getter.
* is enumerable only when the value is even

</details>

<details>
<summary>Task</summary>

* Write a function that takes an object as a single parameter and return array of
  all non-enumerable keys
  * What is type of the returned value?
  
    <details>
    
    ```
    Array<string | Symbol>
    ```
    
    </details>

</details>

### Protecting object against change

* `Object.preventExtensions()`
  * effects 
    * new properties can't be added
    * prototype can't be reassigned
  * it cannot be undone
  * `Object.isExtensible()`

  ```javascript
  {
      const obj = { foo: 8 }
      obj.bar = 9
      console.log('isExtensible', Object.isExtensible(obj))
      Object.preventExtensions(obj)
      console.log('isExtensible', Object.isExtensible(obj))
      obj.baz = 10 // silently ignored because "strict mode is not on"
      console.log('obj', obj)
  }
  
  (() =>
  {
      'use strict'
      const obj = { foo: 8 }
      obj.bar = 9
      console.log('isExtensible', Object.isExtensible(obj))
      Object.preventExtensions(obj)
      console.log('isExtensible', Object.isExtensible(obj))
      obj.baz = 10 // this throws TypeError
      console.log('obj', obj)
  })()
  ```
    
* `Object.seal()`
  * effects
    * it prevents extension
    * object properties are non-configurable
  * it cannot be undone
  * `Object.isSealed()`
  
  ```javascript
  {
      const obj = { foo: 8 }
      console.log('isSealed', Object.isSealed(obj))
      Object.seal(obj)
      console.log('isSealed', Object.isSealed(obj))
      delete obj.foo 
      console.log('obj', obj)
  }
  
  (() =>
  {
      'use strict'
      const obj = { foo: 8 }
      console.log('isSealed', Object.isSealed(obj))
      Object.seal(obj)
      console.log('isSealed', Object.isSealed(obj))
      delete obj.foo 
      console.log('obj', obj)
  })()
  ```
    
* `Object.freeze()`
  * effects
  * it prevents extension
  * object properties are non-configurable and non-writable
  * it cannot be undone
  * `Object.isFrozen()`
  
  ```javascript
  {
      const obj = { foo: 8 }
      console.log('isFrozen', Object.isFrozen(obj))
      Object.freeze(obj)
      console.log('isFrozen', Object.isFrozen(obj))
      obj.foo = 9
      console.log('obj', obj)
  }
  
  (() =>
  {
      'use strict'
      const obj = { foo: 8 }
      console.log('isFrozen', Object.isFrozen(obj))
      Object.freeze(obj)
      console.log('isFrozen', Object.isFrozen(obj))
      obj.foo = 9
      console.log('obj', obj)
  })()
  ```

<details>
<summary>Task</summary>

Observe protection against change of objects

```javascript
Object
Object.prototype
globalThis.crypto
```
using methods

```javascript
Object.isExtensible()
Object.isSealed()
Object.isFrozen()
Object.getOwnPropertyDescriptors()
```
    
</details>

<details>
<summary>Task</summary>

Task: is frozen object considered not extensible?

<details>

```javascript
Object.isExtensible(Object.freeze({}))
```

</details>

</details>

## Prototype inheritance

* objects can be organized singly linked list like structure. When a property is being accessed, all
  objects in the chain are searched till the property if found (or `undefined` is returned in case
  of read access and property is created in the first list element in case of write access).
* Object linked as the "following one to search for a property" is called **prototype**
* The linked list of objects is called **prototype chain**
* Properties found in object itself (not in other objects down the chain) are called **own**

![prototype chain](./images/prototype-chain-of-an-array.drawio.svg)

```javascript
{
    const obj = {
        '0': 'hello',
        '1': 'world',
        'foo': 'bar',
    }
    // get prototype
    Object.getPrototypeOf(obj)
    // also a deprecated way
    obj.__proto__
    
    // set prototype - there should never be a good reason for that, very slow
    Object.setPrototypeOf(obj, Array.prototype)
    
    // using property that is not own
    obj.toString()
    
    // checking for property presence
    '1' in obj
    'toString' in obj

    // creating object with no prototype
    const noProtoObject = Object.create(null, {
        foo: {
            value: 8
        } })

    // creating object with a specific prototype
    const childObj = Object.create(obj, {
        foo: {
            value: 8
        },
        hare: {
            value: 'agile animal',
            enumerable: true
        }
    })
    
    // iterating through all enumerable properties
    for (const propertyName in childObj) {
        console.log('property', propertyName, 'isOwn', Object.hasOwn(childObj, propertyName))
    }
    
    // properties shadowing
    
    // new value is store as own
    childObj[0] = 'bye'
    
    // change in prototype, very slow, didscouraged
    delete childObj[0]
}
```

### Own stuff

```javascript
{
    const obj = {
        '0': 'hello',
        '1': 'world',
    }
    Object.defineProperties(obj, {
        'foo': { value: 'bar' },
        [Symbol('symbolKey')]: {
            value: 'symbol value',
            enumerable: true,
            writable: true,
            configurable: true
        }
    })

    console.log('foo property descriptor', Object.getOwnPropertyDescriptor(obj, 'foo'))
    console.log('property descriptors', Object.getOwnPropertyDescriptors(obj))
    console.log('own property names', Object.getOwnPropertyNames(obj))
    console.log('own property symbols', Object.getOwnPropertySymbols(obj))
    console.log('own property symbols of an array prototype',
        Object.getOwnPropertySymbols(Object.getPrototypeOf([3, 4])))
    
    // own enumerabele string-based
    console.log('keys()', Object.keys(obj))
    console.log('values()', Object.values(obj))
    console.log('entries()', Object.entries(obj))


    console.log('hasOwn(\'foo\')', Object.hasOwn(obj, 'foo'))
    console.log('hasOwn(\'toString\')', Object.hasOwn(obj, 'toString'))

    console.log('hasOwnProperty(\'foo\')', obj.hasOwnProperty('foo'))
    console.log('hasOwnProperty(\'toString\')', obj.hasOwnProperty('toString'))

    console.log('obj.hasOwnProperty(\'hasOwnProperty\')',
        obj.hasOwnProperty('hasOwnProperty'))
    console.log('Object.getPrototypeOf(obj).hasOwnProperty(\'hasOwnProperty\')',
        Object.getPrototypeOf(obj).hasOwnProperty('hasOwnProperty'))
    
}
```

<details>
<summary>Task</summary>

Write a function that

* takes an object as an argument
* returns array of arrays of keys (string or Symbol) of the object grouped by prototypes. First
  array contains argument's own properties.

Verify on inputs

```javascript
Object.create(null)
Object.assign(Object.create(null), { foo: 4, bar: 5 })
({ foo: 4, bar: 5 })
Object.assign(Object.create([3, 4]), { foo: 4, bar: 5 })
Object.assign(Object.create([3, 4]), { foo: 4, bar: 5 })
Object.assign(Object.create(Object.create(Object.create([3, 4]))), { foo: 'bar' })
Number(7)
666
```

<details>

```javascript
function inspect(object) {
    if (typeof object !== 'object') {
         throw new Error('not an object ' + object) 
    }
    let o = object
    let result = []
    while (o) {
        result.push(Object.keys(Object.getOwnPropertyDescriptors(o)))
        o = Object.getPrototypeOf(o)
    }
    return result
}

console.log(inspect(Object.create(null)))
console.log(inspect(Object.assign(Object.create(null), { foo: 4, bar: 5 })))
console.log(inspect(({ foo: 4, bar: 5 })))
console.log(inspect(Object.assign(Object.create([3, 4]), { foo: 4, bar: 5 })))
console.log(inspect(Object.assign(Object.create([3, 4]), { foo: 4, bar: 5 })))
console.log(inspect(Object.assign(Object.create(Object.create(Object.create([3, 4]))), { foo: 'bar' })))
console.log(inspect(Number(7)))
console.log(inspect(666))
```

</details>

</details>

## Object creation

```javascript
{
    const literal = {
        foo: 'bar'
    }
    console.log('literal', literal)
    
    const objCreate = Object.create(literal, {
        foo: {
            value: 'baz',
            enumerable: true,
            writable: true,
            configurable: true
        },
        property2: {
            value: 'value2',
            enumerable: true,
            writable: true,
            configurable: true
        }
    })
    console.log('objCreate', objCreate)
    console.log('objCreate owns', Object.getOwnPropertyDescriptors(objCreate))
    
    function Obj() {
        this.foo = 'bar'
    }
    const fromConstructor = new Obj()
    console.log('fromConstructor', fromConstructor)
    
    const fromEntries = Object.fromEntries([['foo', 'bar'], ['foo', 'baz'], ['hello', 'world']])
    console.log('fromEntries', fromEntries)
}
```

### Bulk properties copying

`Object.assign()` copies **own enumerable** properties

```javascript
{
    const parent = {parentProperty: 'parentPropertyVaue'}
    const source = Object.create(parent, {
        foo: {
            value: 'bar',
            enumerable: true,
            writable: true,
            configurable: true
        },
        other: {
            value: 'bar',
            enumerable: true,
            writable: true,
            configurable: true
        },
        notEnumerable: {
            value: 'bar',
            enumerable: false,
            writable: true,
            configurable: true
        }
    })
    console.log('source', source)

    const fromAssign = Object.assign({}, source, {other: 'baz'})
    console.log('fromAssign', fromAssign)
}
```

### Object member properties

* `Object.prototype.toString()`

  ```javascript
  {
      const obj = { foo: 'bar'}
      
      console.log('obj', obj)
      console.log('obj.toString', obj.toString)
      console.log('obj.toString === Object.prototype.toString', obj.toString === Object.prototype.toString)
      console.log('obj.toString()', obj.toString())
      console.log('"" + obj', "" + obj)
      
      obj[Symbol.toStringTag] = 'custom toString tag'
      console.log('obj.toString()', obj.toString())
  }
  ```

## Function

### Function creation

```javascript
{
    // function literal
    function plus(a, b) {
        return a + b
    }
    plus(1, 2)

    const minus = function (a, b) { 
        return a - b
    }
    minus(1, 2)

    // lambda, arrow function
    const times = (a, b) => a * b
    times(1, 2)

    // constructor
    const div = new Function('a', 'b', 'return a / b')
    div(1, 2)
}
```

* scope of creation

  ```javascript
  {
      function plus(a, b) {
          return a + b
      }
      console.log('globalThis.plus === plus', globalThis.plus === plus)
  
      function hypotenuse(a, b) {
          // local function
          function sq(a) {
              return a ** 2
          }
          return Math.sqrt(sq(a) + sq(b))
      }
  
      function leg(h, a) {
          {
              // local definitions are function scoped
              function sq(a) {
                  return a ** 2
              }
          }
          return Math.sqrt(sq(h) - sq(a))
      }
  }
  ```
  
* Argument default values

  ```javascript
  {
      // number of declared and passed arguments doesn't need to match
      function a(foo, bar) {
          console.log('foo', foo, 'bar', bar)
      }
      console.log('a:')
      a()
      a(1, 2, 3)
  
      // only variables on the left can be referenced
      function b(foo, bar = 8, baz = foo) {
          console.log('foo', foo, 'bar', bar, 'baz', baz)
      }
      console.log('b:')
      b()
      b(1)
      b(1, undefined)
      b(1, 2)
      b(1, 2, 3)
  
      // equivalent to assigning when undefined
      function c(foo) {
          if (foo === undefined) {
              foo = 'def'
          }
          console.log('foo', foo)
      }
      console.log('c:')
      c()
  
      const l = (foo = 8) => {
          console.log('foo', foo)
      }
      console.log('l:')
      console.log(l())
  }
  ```

* Rest parameters

  A.k.a. varargs

  ```javascript
  {
      function a(foo, bar, ...baz) {
          console.log('foo', foo, 'bar', bar, 'baz', baz)
      }
      a()
      a(1)
      a(1, 2)
      a(1, 2, 3)
      a(1, 2, 3, 4)
  }
  ```
  
<details>
<summary>Task</summary>

Create a method `log()` such that

* it accepts first argument `severity` of type `"debug" | "info" | "warn" | "error"`
* followed by arbitrary number of messages that will be printed using potentially multiple calls
  of appropriate (according to the first argument) `console` method.
* if no message is set at call site, single message `"default message"` is assumed

<details>

```javascript
function log(severity, ...messages) {
    const allowedSeverities = ['debug', 'info', 'warn', 'error']
    if (!allowedSeverities.includes(severity)) {
        throw new Error('unknown severity ' + severity)
    }
    if (messages.length === 0) {
        messages.push('default message')
    }
    for (const message of messages) {
        console[severity](message)
    }
}

log('debug')
log('error', 'baz')
log('warn', 8, 'foo')
log('foo')
```

</details>

</details>

* Property `name`

  ```javascript
  {
      function foo() {}
      console.log('foo.name', foo.name)
      
      const bar = function() {}
      console.log('bar.name', bar.name)
      
      const baz = () => {}
      console.log('baz.name', baz.name)
      
      console.log('no name', (function () {}).name)
      console.log('no name lambda', (() => {}).name)
  }
  ```

* Property `length`

  Number of declared arguments

  ```javascript
  {
      function a() {}
      console.log('a.length', a.length)
  
      function b(foo, bar) {}
      console.log('b.length', b.length) 
  
      const c = (foo) => {}
      console.log('c.length', c.length)
  
      console.log(((foo, bar = 8, baz) => {}).length)
      console.log(((foo, ...bar) => {}).length)
  }
  ```

end of previous session
<hr>
  
### `this`
  
```javascript
{
    // outside a function or inside just a lambda
    console.log(this === globalThis)
    (() => console.log(this === globalThis))()

    // inside a function non-strict mode
    // if function not bound (Function.prototype.bind), bound to `undefined` or `null` -> globalThis
    // if function bound to a primitive value -> an object wrapper of that value
    // if function bound to an object -> that object
    // i.e. the value is always an object
    function foo() {
        console.log('this in foo()', this)
    }

    foo()
    foo.call('bar')
    foo.call({bar: 'baz'})

    // inside a function strict mode
    // `undefined` or the bound value
    function foo() {
        'use strict'
        console.log('this in foo()', this)
    }

    foo()
    foo.call('bar')
    foo.call({bar: 'baz'})

    // inside a constructor
    {
        let thisFromConstructor

        function Foo(label) {
            this.label = label
            console.log('this in foo()', this)
            thisFromConstructor = this
        }

        const newObj = new Foo('baz');
        console.log(newObj)
        console.log('newObj === thisFromConstructor', newObj === thisFromConstructor)
    }

    // inside a method
    {
        const obj = {
            foo() {
                console.log('foo method', this)
            },
            'bar': function () {
                console.log('foo method', this)
            },
            'baz': () => console.log('baz lambda', this, 'this === globalThis', this === globalThis)
        }
        console.log(obj)
        console.log(obj.foo())
        console.log(obj.bar())
        console.log(obj.baz())
    }
}
```

#### Scoping of `this`

```javascript
{
    const outsideFunctionThis = this
    const obj = {
        a() {
            const aThis = this
            const nestedObj = {
                b() {
                    const bThis = this
                    console.log(outsideFunctionThis, aThis, bThis)
                }
            }
            nestedObj.b()
        }
    }
    obj.a()
}
```

#### Value of `this` depends on way the function / method is called

... rather than on way it's defined

```javascript
{
    const lambda = () => this

    function standaloneFunction() {
        'use strict'
        return this
    }
    
    const obj = {
        method() {
            'use strict'
            return this
        }
    }
    
    console.log(lambda(), standaloneFunction(), obj.method())
    
    obj.lambdaAsMethod = lambda
    console.log('obj.lambdaAsMethod()', obj.lambdaAsMethod())
    
    obj.standaloneFunctionAsMethod = standaloneFunction
    console.log('obj.standaloneFunctionAsMethod()', obj.standaloneFunctionAsMethod())
    
    const methodAsStandaloneFunction = obj.method
    console.log('methodAsStandaloneFunction()', methodAsStandaloneFunction())
}
```

### Function as a constructor

```javascript
function Cat(name) {
    console.log('constructor begin', this)
    this.tag = "Cat " + name
    console.log('constructor end', this)
}

{
    const tom = new Foo('Tom')
    console.log(tom)
}
```

What happens:
1. an object is created
2. it's `[[prototype]]` property is set to `Cat.prototype`
3. the constructor is called, the new object is bound to `this`

#### Constructor return value

* don't use explicit return
* constructor call value
  * a primitive value or `null` returned -> `this`
  * an object returned -> that object

```javascript
console.log(new (function () { this.foo = 'bar' ; return 3 })())
console.log(new (function () { this.foo = 'bar' ; return [1, 2] })())
```

#### Keyword `function` has to be used for class to be usable as a constructor

* ... or keyword `class` or `new Function(string)`
* TypeError thrown
* functions  prototype property


```javascript
{
    const lambda = () => {}
    console.log(lambda)
    console.log(new lambda()) // TypeError: lambda is not a constructor
}
{
    const obj = {
        a() {},
        b: function () {}
    }
    console.log('o.a', o.a, 'o.b', o.b)
    console.log('new o.b()', new o.b())
    console.log('new o.a()', new o.a()) // TypeError: o.a is not a constructor
}
```

### `<functionInstance>.prototype`

* A property created for functions usable as constructors, i.e. defined using `function` or `class` keywords
  or `new Function(string)`
* It is used as a prototype of object that are constructed when the function is invoked as a constructor
* `Object.getPrototypeOf(<functionInstance>)` vs `<functionInstance>.prototype`
* It allows for properties shared among all instance - suitable for methods
* Prototype of `<functionInstance>.prototype` points to the parent in inheritance hierarchy 

```javascript
function Foo() {
    this.foo = 'Hello '
}

Foo.prototype.bar = function(name) {
    return this.foo + name
}

{
    const instance = new Foo()
    console.log('instance.bar()', instance.bar())
    
    // Own properties vs properties from prototype
    console.log(instance)
    console.log('Object.hasOwn(instance, \'foo\')', Object.hasOwn(instance, 'foo'))
    console.log('Object.hasOwn(instance, \'bar\')', Object.hasOwn(instance, 'bar'))
    console.log(Object.getPrototypeOf(instance) === Foo.prototype)
    console.log(Object.getPrototypeOf(Object.getPrototypeOf(instance)) === Object.prototype)
    console.log(Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(instance))) === null)
}
```

* Change of `<functionInstance>.prototype` affects existing objects as well

```javascript
function Foo() {}

{
    const instance = new Foo()
    console.log('before prototype altered', instance)
    Foo.prototype.bar = function() { console.log('bar') }
    console.log('after prototype altered', instance)
    instance.bar()
    ;(new Foo()).bar()
}
```

### `Object.prototype.constructor`

* provided the object inherits from `Object`
  * a reference to the constructor function when constructed using a constructor
  * reference to `Object`

```javascript
console.log({}.constructor)

function Foo() {}
console.log((new Foo()).constructor)

function Bar() {}
Bar.prototype = Object.create(null)
Bar.prototype.baz = function () {}
console.log((new Bar()).constructor)
```

<details>
<summary>TASK</summary>

Implement [`Array.prototype.with()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/with).

It's more fun in Firefox since it hasn't implemented it yet.

<details>

```javascript
Array.prototype.with = function (index, value) {
    const result = []
    for (const val of this) {
        result.push(val)
    }
    result[index] = value
    return result
}

Array.prototype.with = function (index, value) {
    const result = this.slice()
    result[index] = value
    return result
}
```

</details>

</details>

### `Function.prototype.toString()`

* it returns sourcecode of user defined function
* `... [native code] ...` for standard library or runtime defined functions

```javascript
Object.prototype.hasOwnProperty.toString()
function Foo() { console.log('bar') }
Foo.toString()
```

### `new.target` meta-property

* in function called as constructor it references the constructor function
* otherwise `undefined`

```javascript
function foo() {
    console.log(new.target)
}

foo()
new foo()
```

<details>
<summary>TASK</summary>

Create a function that prints using `console.log()` the context it is called in:

* `constructor`
* `function`
* `method`

<details>

```javascript
function foo() {
    'use strict'
    if (new.target !== undefined) {
        console.log('constructor')
        return
    }
    if (this === undefined) {
        console.log('function')
        return
    }
    console.log('method')
}

new foo()
foo()
obj = { foo }
obj.foo()
```

</details>

</details>

### `instanceof`

* operator looking for `<functionInstance>.prototype` in object's prototype chain by default
* customizable by `Symbol.hasInstance`

```javascript
({}) instanceof Object
;[1] instanceof Array
;[1] instanceof Object
Object instanceof Function
Object instanceof Object

function Foo() {}
;(new Foo()) instanceof Foo
;(new Foo()) instanceof Object
```

### `Symbol.hasInstance`

```javascript
function Foo(bar) { this.bar = bar }

Object.defineProperty(Foo, Symbol.hasInstance, {
    value(instance) {
        console.log('Foo.@@hasInstance called with', instance)
        return (typeof instance.bar) === 'string'
    },
});

new Foo('baz') instanceof Foo
new Foo(8) instanceof Foo
```

<details>
<summary>TASK</summary>

Write a constructor function `Foo` that
* creates an object `{ a: 8 }`
* and it denies it's origin, it pretends it was created using object literal

<details>

```javascript
function Foo() { this.a = 8 }

Object.defineProperty(Foo, Symbol.hasInstance, {
    value(instance) {
        return false
    },
});

Foo.prototype.constructor = Object

console.log(new Foo(), new Foo() instanceof Foo)
```

</details>

</details>

### Comparison of Java & Javascript OOP primitives

#### Constructor & public field

Java:
```java
public class Foo {
    public String bar;
    
    public Foo(String bar) {
        this.bar = bar;
    }
}
```

Javascript
```javascript
function Foo(bar) {
    this.bar = bar
}
```

#### Public method

Java:
```java
public class Foo {
    public void bar() {}
}
```

Javascript:
```javascript
function Foo() {}

Foo.prototype.bar = function() {}
```

#### Static properties

Java:
```java
public class Foo {
    public static int bar = 3;
    
    public static int getBarPlusTwo() {
        return Foo.bar + 2;
    }
}
```

Javascript:
```javascript
function Foo() {}

Foo.prototype.bar = 3
Foo.prototype.getBarPlusTwo = function () {
    return Foo.prototype.bar + 2
}
```

#### No counterpart

* class visibility
* private, protected properties

<details>
<summary>TASK</summary>

Write in Javascript:

```java
public class Counter {
    public static String COUNTER = "Counter";
    
    public int value;
    
    public Counter(int value) {
        this.value = value;
    }
    
    public void decrement() {
        if (value < 0) {
            throw new RuntimeException("Value is already 0");
        }
        this.value--;
    }
    
    public String toString() {
        return COUNTER + " " + value;
    }
    
    public static Counter createTenCounter() {
        return new Counter(10);
    }
}
```

</details>

<hr>

* object prototype
    * chain
    * own properties, symbols
    * separation of
      * "instance properties" - fields, 
      * "class properties" - methods, 
      * "constructor properties" - static methods and fields
      * discussion on "private properties emulation"
        * creating function with closure
        * `_` prefix
* `Object.prototype.toString()`
* Function
    * named and anonymous
    * functions are first class values
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
    * `Function.prototype.toString()`
    * example of inheritance
      * bullet list on how to do it
    * lambda, arrow function
      * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
      * differences
        * no constructor
        * no `yield`
        * no `this`, `arguments`, `super`
        * no `prototype`
    * `Object.prototype.constructor`
    * `instanceof` operator
        * `Symbol.hasInstance`
    * `bind`, `apply`, `call`
      * lambda
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
        * `this` not bound, see above
    * immediately invoked function