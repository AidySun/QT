# QFace

## Grammer

- module
- interface
- struct
- enum
- flag
  - bit mask of enum, values are in the sequence of the 2^n

  ```
  module <mudule> <version>
  import <module> <version>
  interface MyInterface {
  	<type> <identifier>
  	<type> <operation>(<parameter>*)
  	signal <signal>(<parameter>*)
  }

  @extends: MyInterface
  interface MySubInterface {
  	real age;
  }
  ``` 

## Types

- string
- int
- real
- list: an array of provided value type
- map: specifics only value type
- model
  - a special type of `list`. add/remove/change
  - `model` should be used if the data is expected to grow infinitely

- default values (text strings) are supported to be assigned to interface properties and struct fields.
  ```
  interface a {
  	int i = "100";
  }
  ```

















