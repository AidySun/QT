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

- convert number to boolen
  - except `NaN` and `0`, all others are `true`
- string -> boolean
  - `""` empty string is false, others are true
- `Boolead(null), Boolead(undefined)` all are `false`
- `Object` can be convert to be `true` (*TBD*)
























