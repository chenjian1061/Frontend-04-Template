学习笔记
========
JS表达式
========
1.运算符和表达式
---------------
* Atom
  * Grammar
    * Grammar Tree vs Priority
    * Left hand side & Right hand side
  * Runtime
    * Type Convertion
      * a+b
      * "false"==false
      * a[o]=1;
      
      ||Number|String|Boolean|Undefined|Null|Object|Symbol
      |-----|-----|-----|-----|-----|-----|-----|-----|
      |Number|- | |0 false |X |X |Boxing |X |
      |String| |- |"" false |X |X |Boxing |X |
      |Boolean|true 1 false 0 |'true' 'false' |- |X |X |Boxing |X |
      |Undefined|NaN |'Undefined' |false |- |X |X |X |
      |Null|0 |'null' |false |X |- |X |X |
      |Object|valueOf |valueOf toString |true |X |X |- |X |
      |Symbol|X |X |X |X |X |Boxing |- |
      
    * Reference
      * Ojbect
      * Key
      * delete
      * assign
* Expresion
  * Member
    * a.b
    * a[b]
    * foo`string`
    * super.b
    * super[ `b` ]
    * new.target
    * new Foo()
  * New
    * new Foo
    ```javascript
    new a()()
    new new a()
    ```
  * Call
    * foo()
    * super()
    * foo()['b']
    * foo().b
    * foo()`abc`
    ```javascript
    new a()['b']
    ```
  * Left Handside & Right Handside
    ```javascript
    a.b = c;
    a_b = c
    ```
  * Update
    * a++
    * a--
    * --a
    * ++a
    ```javascript
    ++a++;
    ++(a++)
    ```
  * Unary
    * delete a.b
    * void foo()
    * typeof a
    * +a
    * -a
    * ~a
    * !a
    * await a
  * Exponental
    *  \*\*
    ```javascript
    3 ** 2 ** 3
    3 ** (2 ** 3)
    ```
  * Multiplicative
    * \* / %
  * Additive
    * + -
  * Shift
    * << >> >>>
  * Relationship
    * < > <= >= instanceof in 
  * Equality
    * ==
    * !=
    * ===
    * !==
  * Bitwise
    * & ^ |
  * Logical
    * &&
    * ||
  * Conditional
    * ?:
    
* Statement
* Stucture
* Program/Module
