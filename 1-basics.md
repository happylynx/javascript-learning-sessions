# Javascript 1

## Lullaby

### History

* created by Brendan Eich at Netscape in 1995 in two weeks
* standardized as ECMAScript https://tc39.es/ecma262/
* "JavaScript" trademark owned by Oracle (previously Sun)
* versions:
  * 3 (1999)
  * 4 (abandoned in 2003)
    * many parts incorporated into Macromedia's Flash scripting language ActionScript
    * features
      * classes
      * modules
      * type annotations & static typing
      * generators & iterators
      * destructuring assignment
      * algebraic data types
  * 5 (2009)
    * strict mode added
    * `JSON` added
    * many Array methods
  * 6 (2015)
    * aka ES2015
    * https://kangax.github.io/compat-table/es6/
  * ... annual updates

* user friendly reference
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference
  * compatibility table
  * specification link
* no reference implementation, multiple environments
  * web browsers
  * server-side
    * node.js
    * deno
    * bun
  * Java
    * https://www.graalvm.org/22.0/reference-manual/js/JavaInteroperability/
  * Oracle DB
    * https://medium.com/graalvm/mle-executing-javascript-in-oracle-database-c545feb1a010#55f2

## Data types

<details>
    <summary>Find primitive data types</summary>

... using `typeof` operator

```javascript
""
'foo ' + 2.0
33
2 ** -12
33n
`foo 
 ${new Date()} bar`
Symbol('foo')
Symbol.for('bar')
new Symbol('baz')
{}
true
{}['foo']
null
!!undefined
[3, 'apes']
new String('foo')
Boolean(null)
NaN
BigInt('-1_024')
```
</details>

### `boolean`

* instantiable wrapper

  ```javascript
  new Boolean(true) !== true // true
  typeof new Boolean(false)
  typeof false
  ```

* autoboxing

  ```javascript
  (true).toString()
  ```
* to primitive value

  ```javascript
  new Boolean(true).valueOf()
  ```
  
* "truthy"
  * all values except for `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`
    
    ```javascript
    if ('foo') {
        console.log('bar')
    }
    ```
    
  * explicit conversion `!!`


### `number`

* instantiable wrapper
* 64bit floating point
* bitwise operations are performed on 32 bit integers
* collection of static methods `Math`
* literals
  ```javascript
  -12
  1.602e-19
  0o72 // 58
  0x10 // 16
  0b1101 //13
  ```
* special values
  * Infinity, NoN
  
  ```javascript
  -1/0
  Number.parseInt('foo')
  ```

### `string`

* instantiable wrapper
* literals
  * `''`
  * `""`
  * ```javascript
    const foo = 'bar'
    `<b>
    foo=${foo}
    </b>`
    ```

### `symbol`

* a bit like uniques strings

```javascript
Symbol('a') === Symbol('a') // false
```

* runtime-wide symbols

```javascript
Symbol.for('a') === Symbol.for('a') // true
```

* well known symbols
  * `Symbol.toStringTag`, `Symbol.iterator`, ...
  * key role in customization of object behavior

### `bigint`

* unlimited integer
* in supports basic operations `+`, `*`, `-`, `**`, `%` and bitwise operators except for `>>>`

```javascript
1234n ** 5678n
```

### `object`

* dynamic, a bit like HashMap, dictionary
* keys can only be strings and symbols

```javascript
{
    const propertyName = 'baz'
    const symbolA = Symbol('a')
    const o = {
        foo: 'hello',
        'bar': 8,
        [propertyName]: [],
        [symbolA]: 9
    }
    console.log(o)
    o[Symbol.toStringTag] = 'My great object'
    delete o.baz
    console.log(o)
}
```

### `undefined`

* "empty" value
  
  ```javascript
  {
      const a = { foo: 'bar' }
      console.log(a.baz) // undefined
  }
  ```
  
* default return value

  ```javascript
  function foo() {
      return
  }
  console.log(foo())
  ```
  
* `void` operator

  ```javascript
  void parseInt('3')
  ```

### Null

* typically used to express empty result

```javascript
typeof null === 'object'
```

## Variables

### Definition

* `var`, `const`, `let`
  * const value, not properties
* hoisting
* use before initialization
* default value `undefined`

```javascript
function a(flag) {
    'use strict'
    if (flag) {
        foo = 'bar'
    }
    var foo
    console.log(foo)
}
```

```javascript
function a(flag) {
  'use strict'
  if (flag) {
    var foo
  }
  foo = 8
  console.log(foo)
}
```

```javascript
function a(flag) {
    'use strict'
    var foo
    var foo
}
```

### Scopes

* local
* enclosing
  * block
  * function
  * module
* global

<details>
<summary>Write function <code>createLogger(loggerPrefix: string): (string) => void</code></summary>
</details>


* global object
    * `globalThis`
      * `window`, `global`, 
      
      ```javascript
      console.log(globalThis)
      
      {
          let counter = 0
          for (const i in globalThis) {
              counter++
          }
          console.log('enumerable global properties', counter)
      }
      ```
    * full of objects already there
      
      ```javascript
      globalThis.Math === Math // true
      ```
    * ```javascript
      delete Math
      ```

## Flow control

* `if-else`
  * ternary operator
* `while`
  * `continue`, `break`
  * labels
    
    ```javascript
    function a() {
        let counter = 0
        outter:
        while (true) {
            while (true) {
                counter++
                console.log('counter', counter)
                if (counter > 3) {
                    break outter
                }
            }
            console.log('never printed')
        }
    }
    a()
    ```
* `do-while`
* `for`
  * comma operator
* `for ... in`
* `switch`
* `try-catch-finally`

## Semicolons

* not required (most of the time), automatically inserted
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#automatic_semicolon_insertion
* rules of thumb
  * never break line after `return`, `throw`, `yield`
  * always put semicolon before `(`, `[`, `,` `+`, `-`, `/` 

## Comments

```javascript
#!/usr/bin/node
// 👆 likely not yet official

// single line

/*
multi
line
 */

/**
 * JSDoc
 */
function foo() {
  
}
```

## Debugging

### `console`

### `debugger`

## TODO 

* casting vs coercion
* equality
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness