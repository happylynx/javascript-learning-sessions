# Classes

## Defining classes

* mostly a syntactic sugar to functions and prototype inheritance
* class declaration
  * as if defined using `let`
    * it can't be redeclared
    * block scoped
* class expression
* not hoisted - temporal dead zone, class can't be used before it's declaration
* body of classes is always run in strict mode
* class elements categories:
  * getter & setter, method, field
  * static, instance
  * public, private
  * special elements
    * constructor
    * class initialization block
* elements overriding
  * not allowed for private elements and constructor

### Static initialization block

* all static elements are evaluated in order
* can be multiple of them

### Static method & fields

* can only be reference using class name, not an instance name
* `this` in a static method returns reference to constructor function
  * `undefined` when not called on a function

### Constructor

* it can't be called without `new`, i.e. as a regular function

### Methods

### Fields

* created in constructor or declared explicitly

### Private properties

* can only be referenced from inside the class
* fields has to be declared
* can't be deleted
* computed name

## Inheritance

* in constructor `super()` has to be called before `this` keyword can be used
* even if parent class has a constructor with parameters defined, child class doesn't have to explicitly declare
  a constructor
