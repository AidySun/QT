# JavaScript

## Data Types

- Six kinds:
  - String
  - Number
  - Boolean
  - Null
  - Undefined (1~5 are basic data types)
  - Object (reference type)

- `typeof varA1` : check type of var
- `Infinity` shows the number value larger than `Number.MAX_VALUE`
  - `var a = -Infinity; typeof a; // number`

- `NaN` : not a number
  - `typeof NaN;` // is "number"
- `null`: empty object
  - `typeof null;` // object
- `undefined`: declared but not initialized
  - `var b; console.log(b);` // "undefined"
  - `typeof b;` // "undefined" as well

### type conversion

#### convert other types to string

1. `toString()` if var is `String, Number, Boolead`
  ```
  var a = 123;
  console.log(a.tostring());
  ```
  - `null` and `undefined` don't have `toString()` method, exception would happen

2. `String()` can be used for all types (including `null` and `undefined`) to convert to string

#### convert to number

1. `Number(varName)`
  - `NaN` would be returned if convertion failed
  - `var a = "    "; var b = Number(a); ` // b would be 0
  - `var c = Number(null);` // c == 0
  - `var d = Number(undefined);` // d == NaN

2. `parseInt(), parseFloat()` : convert valid value in string to be Int/Float
  - `a = "123px"; a = parseInt(a);` // a == 123 and typeof is number
  - `parseInt(var, base)`
    - `var s = "070"; var intS = parseInt(var, 10)`

#### convert to boolean

- Number -> boolen
  - `NaN` and `0` are false
- String -> boolean
  - `""` empty string is false, others are true
- `Boolean(null), Boolean(undefined)` all are `false`
- `Object` -> boolean is `true`

#### Math Operator

- `+` 
  - any type + with string, it will be converted to be string
    - `var c = 100 + '1';` // c== "1001"
- besides `+`, other operators (`- * /`) with string would convert string to number
  - `var d = 100 - '1';` // d == 99
  - `var f = "123"; f = f - 0;` // typeof f == Number, f == 123
- any value operate with `undefined` is `NaN`
- `var e = 23 * null;` // var == 0

## Object

### Diff v.s. basic types

- Object is reference type, basic types are value type
  - `var obj = new Object(); var obj2 = obj; obj.name="tom"; // obj2.name == "tom"`
- `operator ==` 
  - Object compares address
  - Basic types compare value
- Diff store ares
  - Object is in heap, while basic types are in stack

### declare object var

1. `var obj = new Object();` 
2. `var obj1 = {name:"tom", age:18, subObj: {tel:"122"} };`
  - key is a string, it can have double quotes, e.g. `var obj2 = { "name": "Jerry" };`


## Function

- Function is an object as well
  - `var fun = new Function(" console.log('hello'); "); ` // typeof fun is function
  - `fun(); fun.name="i am a function";`

- anonimous function
  ```
  var funObj = function() {
  	alert('hi');
  };
  funObj();
  ```

- function declare
  ```
  function func1() {

  }
  ```
- immediat execution function
  ```
  (function(a) { alert(a); })("hello world");
  ```

- function doesn't check parameter types
- function doesn't check parameter count

- get all properties of an object
  ```
  for (var p in objVar) {
  	console.log(p + ": " + objVar[p]);
  }
  ```

## Global var / Window

- `window` is a global var

- `function` execution can be in front of the declaration. `var` cannot.
  - function are parsed before code execution.

- in function, if variable is not declared with `var` keyword, it would be the globle scope
  ```
  function a() {
  	c = 100; // same as window.c = 100;
  }
  alert(c); // 100
  ```

## this

- context object of parent scope that invoking the function

- constructor function
  ```
  function Person(name) {
  	this.name=name;			// this is the invoker
  }
  var p1 = new Person("tom");  // this in Person() is `p1`
  ```

## Prototype

- Each functin has a prototype, it is an object
  ```
  function MyClass() {

  }

  var mc = new MyClass();
  console.log(mc.__proto__ == MyClass.prototype); // "true"
  ```


- `in` / `hasOwnProperty` to check property in object
  ```
  function MyClass() {
  	this.age = 12;
  }
  MyClass.prototype.name = "name in prototype";

  var mc = new MyClass();
  if ("name" in mc) { // true, since prototype has `name`
  	alert(object.name);
  }
  mc.hasOwnProperty("name");  // false
  mc.hasOwnProperty("age");	// true
  ```

- `__proto__` is an object as well, it also has __proto__
  - untill `Object`, it doesn't have prototype

## GC

JS has automatically GC. Set object variable to be `null` to release its resource.












