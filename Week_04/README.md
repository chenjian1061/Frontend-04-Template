# 学习笔记

# 语法

## 1. 泛用语言分类语法

1. 非形式语言

  - 中英文等

2. 形式语言

  乔姆斯基谱系（从**下**到**上**是包含关系：比如3一定是符合2、1、0的文法。但反之是不成立的）：

  - 0型文法（无限制文法或短语结构文法）包括所有的文法。
  - 1型文法（上下文相关文法）生成上下文相关语言。
  - 2型文法（上下文无关文法）生成上下文无关语言。
  - 3型文法（正规文法）生成正则语言。

## 2. 产生式

产生式：在计算机中指`Tiger`编译器将源程序经过**词法分析（Lexical Analysis）**和**语法分析（Syntax Analysis）**后得到的一系列符合文法规则（Backus-Naur Form，BNF）的语句

巴科斯诺尔范式：即巴科斯范式（英语：Backus Normal Form，缩写为 BNF）是一种用于表示**上下文无关文法**的语言，上下文无关文法描述了一类形式语言。它是由约翰·巴科斯（John Backus）和彼得·诺尔（Peter Naur）首先引入的用来描述计算机语言语法的符号集。

终结符：最终在代码中出现的字符（ https://zh.wikipedia.org/wiki/ 終結符與非終結符）

四则运算产生式：

``` html
<Expresion> ::= <AddtiveExpression> | "("<AddtiveExpression>")"

<AddtiveExpression> ::=
  <MultiplicationExpression>
  | <AddtiveExpression>"+"<MultiplicationExpression>
  | <AddtiveExpression>"-"<MultiplicationExpression>
  | <AddtiveExpression>"+""("<Expresion>")"
  | <AddtiveExpression>"-""("<Expresion>")"

<MultiplicationExpression> ::=
  <Number>
  | <MultiplicationExpression>"*"<Number>
  | <MultiplicationExpression>"/"<Number>
  | <MultiplicationExpression>"*""("<Expresion>")"
  | <MultiplicationExpression>"/""("<Expresion>")"
```

## 3. 语言的分类

根据用途分类：
 - 数据描述语言，如：JSON HTML SQL CSS
 - 编程语言，如：C C++ JAVA Lisp 汇编 VB JS python Go

根据表达方式：
 - 声明式，如：JSON HTML SQL CSS Lisp
 - 命令型，如：C C++ JAVA 汇编 VB JS python Go

## 4. 编程语言的性质

### 4.1 图灵完备性

- 命令性 ———— 图灵机，eg：`goto`、`if-else`、`while`
- 声明式 ———— lambda，eg: `递归`

### 4.2 动态与静态

- 动态，执行编译时检查数据类型 runtime
- 静态，开发编译时检查数据的类型 complietime

### 4.3 类型

1. 动态类型系统与静态类型系统

- 动态类型系统：变量是一个指向存储在内存中的值的名称，通常说的类型是值的类型，而不是变量的类型。
- 静态类型系统：变量具有固定的类型，指向内存中的值。

2. 强类型与弱类型

- 强类型，不具备隐式转换，必须强制转换
- 弱类型，具备隐式转换

3. 复合类型

结构体、函数签名

4. 子类型
5. 泛型：[协变与逆变](https://jkchao.github.io/typescript-book-chinese/tips/covarianceAndContravariance.html)

## 一般命令式语言

1. Atom 原子，变量、操作符
2. Expression 表达式，四则、与或逻辑
3. Statement 语句，if、while
4. Structure 结构，函数体
5. Program 模块，package module lib

# JS 类型

7种：number、boolean、string、undefined、null、symbol、object

## Number

双精度存储。

`0.1 + 0.2 !== 0.3` -> `0.1`和`0.2`用二进制表示将产生无限循环，相加截取精度后不等于`0.3`。

可改为`0.1+0.2 - 0.3 <= Number.EPSILON`

- 2进制 `0b`
- 8进制 `0o`
- 10进制 `10`、`.1`、`10.`、`1e2`  
- 16进制 `0x`

`0.toString()` -> `0.`被识别成数字0，因此会不合法。可改为`0 .toString()`或者`0..toString()`

## String

character（字符） -> code point(ASCII码127个字符) -> 字节（编码）

code point（都兼容ASCII，其他互不兼容）：

1. ASCII码 
2. unicode（000 -> fff）
3. GB（国标）
4. ISO-8859
5. BIG5

encode：

1. UTF-8
2. UTF-16

``` JS
function UTF8_Encoding (str) {
  const buffer = []
  for (let i = 0; i < str.length; i++) {
    let code = str[i].charCodeAt()
    let binary = code.toString(2)
    // 补码
    if (code >= 0x00 && code <= 0x7f) {
      // 1个字节 0xxx xxxx 7->127
      buffer.push(code & 0xff) // 此处第一位补1
    } else if (code >= 0x080 && code <= 0x7ff) {
      // 2个字节 110x xxxx  10xx xxxx 11位->2047
      // 最大11位先补前面的，往右移6位，剩5位，不齐补0
      buffer.push(128 | (31 & code >> 6)) // 128: 0b10000000
      buffer.push(128 | (63 & code)) // 63：0b111111
    } else if (code >= 0x0800 && code <= 0xffff) {
      // 3个字节 1110 xxxx  10xx xxxx  10xx xxxx 16位->65535
      buffer.push(224 | (15 & code >> 12)) // 224：0b11100000；15：0b1111
      buffer.push(128 | (63 & code >> 6))
      buffer.push(128 | (63 & code))
    }
  }
  let utf8 = ''
  for (let i = 0; i < buffer.length; i++) {
    console.log(buffer[i])
    utf8 += buffer[i].toString(2)
  }
  return utf8
}
```

## undefined null

`undefined`代表未定义的值。它是一个全局变量，可以在局部作用域中修改值，因此相对来说是不安全的。因此常用`void 0`代替`undefined`。

`null`表示变量已定义，但值为空。

## Object

对象的行为一定是改变对象状态的。

JavaScript 标准里面所有具有特殊行为的对象（属性+原型）：

- `Object`，没有`setPrototypeOf`方法
- `Array`，其`length`属性会随着数组长度而变化
- `String`的正整数属性是下标索引
- `Arguments`，类数组的正整数属性是下标索引
- 模块对象`namespace`，特殊的导出
- `ArrayBuffer`类型化数组，存储的是内存中指定的字节数
- bind 后的 function：跟原来的函数相关联

## Symbol

为什么Symbol()生成的值是唯一的？因为Symbol每次创建的也是一个内存地址。是一种类对象属性。
