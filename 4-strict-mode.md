# Strict mode

## What is strict mode

* Set of opt-in language restrictions to fix design flaws 
* Introduced in ECMAScript 5 (2009)
* It aims to
  * Replace silent errors by throwing exceptions
  * Eliminate code that is difficult to optimize for performance
  * Limit syntax likely used in future version of Javascript

## Changes

### Assigning to undeclared variable

```javascript
'use strict'
foo = 8
```

### Failure of assigning to object property

```javascript

```

## Enabling strict mode

* for whole evaluation context / file
  * first statement is string `'use strict'`
* for a function
  * first statement of the function is string `'use strict'` of `"use strict"`
  * not allowed for functions with rest (vararg), default and destructed arguments
* Enabled by default in modules
* Enabled by default in classes

## Detection

```javascript
(function () { return !this})()
```

```javascript
(function () {
    var isStrict = true
    eval('var isStrict = false')
    return isStrict
})()
```
