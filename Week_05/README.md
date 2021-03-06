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
      
    * Unboxing
      * ToPremitive
      * toString vs valueOf
      * Symbol.toPrimitive
      ```javascript
      var o={
       toString() { return "2" },
       valueOf() { return 1 },
       [Symbol.toPrimitive]() { return 3 }
      }
      
      var x={}
      x[o] = 1
      console.log("x"+o)
      ```
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
  * Grammar
    * 简单语句
      * ExpressionStatement
      * EmptyStatement
      * DebuggerStatement
      * ThrowStatement
      * ContinueStatement
      * BreakStatement
      * ReturnStatement
    * 复合语句
      * BlockStatement
        * [[type]]: normal
        * [[value]]: --
        * [[target]]: --
        ```javascript
        {
        
        }
        ```
      * IfStatement
      * SwitchStatement
      * IterationStatement
        * while()
        * do{} while()
        * for( ; ; )
        * for( in )
        * for( of )
        * for await( of )
      * WithStatement
      * LabelledStatement
        * [[type]]: break continue
        * [[value]]: --
        * [[target]]: label
      * TryStatement
        * [[type]]: return
        * [[value]]: --
        * [[target]]: label
        ```javascript
        try {
        
        } catch() {
        
        } finally {
        
        }
        ```
    * 声明
      * FunctionDeclaration
        * function
      * GeneratorDeclaration
        * function \*
      * AsyncFunctionDeclaration
        * async function
      * AsyncGeneratorDeclaration
        * async function \*
      * VariableStatement
        * var
      * ClassDeclaration
        * class
      * LexicalDeclaration
        * const
        * let
      ```javascript
        // 预处理
        var a = 2;
        void function() {
         a=1;
         return;
         var a;
        }();
        console.log(a);
        
        
        var a=2;
        void function() {
          a = 1;
          return;
          const a;
        }();
        console.log(a);
								
								// 作用域
        var a = 2;
        void function() {
         a=1;
									{
										var a;
									}
        }();
        console.log(a);
        
        
        var a=2;
        void function() {
          a = 1;
          {
												const a;
										}
        }();
        console.log(a);
      ```
  * Runtime
    * Completion Record
      * [[type]]: normal, break, continue, return, or throw
      * [[value]]: 基本类型
      * [[target]]: label 
    ```javascript
    if(x==1)
      return 10;
      // 我们需要一个数据结构来描述语句的执行结果：是否返回了？返回值是什么？等等......
    ```
     
    * Lexical Environment
* Stucture
	* 宏任务
		```javascript
		var x=1;
		var p=new Promise(resolve => resolve());
		p.then(() => x=3);
		x=2;
		```
	* 微任务(Promise)
	* 函数调用(Execution Context)
		```javascript
		import {foo} from "foo.js"
		var i=0;
		console.log(i);
		foo();
		console.log(i);
		i++;
		
		// --------
		import {foo2} from "foo.js"
		var x=1;
		function foo(){
			console.log(x);
			foo2();
			console.log(x);
		}
		export foo;
		
		// --------
		var y=2
		function foo2(){
			console.log(y);
		}
		export foo2;
		```
		* Execution Context
			* code evaluation state
			* Function
			* Script or Module
			* Generator
			* Realm
			* LexicalEnvironment
				* this
				* new.target
				* super
				* 变量
			* VariableEnvironment
				* VariableEnvironment是个历史遗留的包袱，仅仅用于处理var声明。
				```javascript
				{
					let y=2;
					eval('var x=1;');
				}
				
				width({a:1}){
					eval('var x;');
				}
				console.log(x);
				```
		* Environent Record
			* Declarative Environment Records
				* Function Environment Records
				* module Environment Records
			* Global Environment Records
			* Object Environment Records
	* 语句/声明(Completion Record)
	* 表达式(Reference)
	* 直接量/变量/this......
* Program/Module
