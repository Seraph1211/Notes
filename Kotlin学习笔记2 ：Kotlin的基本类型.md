# Kotlin的基本类型

和Java一样，Kotlin也是一种强类型语言，即要求：
1、所有变量都需要先声明、后使用。
2、指定类型的变量只能接受类型与之匹配的值。
强类型语言可以在编译过程中发现源代码的错误，从而保证程序更加健壮。

## 2.1 注释

Kotlin支持单行注释、多行注释、文档注释，且形式与Java相同。

## 2.2 变量

Kotlin中的分隔符、标识符相关规则与Java一致。
Kotlin不强制要求每条语句必须以分号结尾。
一行内有多条独立语句，则除最后一条语句外，前面的语句都要求以分号结尾。
Kotlin允许一条语句跨多行。
Kotlin中的标识符可以包含中文字符、日文字符等。

#### 2.2.3 Kotlin的关键字

Kotlin关键字分为3类：硬关键字、软关键字、修饰符关键字。
硬关键字：无论在什么情况下都不可以作为标识符。
软关键字：可以在他们不起作用的上下文中作为标识符。
修饰符关键字：可以在代码中作为标识符。

#### 2.2.4 声明变量

Kotlin中的变量要求先声明、后使用。格式如下：
var|val 变量名 [: 类型] [= 初始值]
var用于声明变量， val用于声明常量。

声明变量时必须显示或隐式地指定变量的类型：
var a   --> 非法
var a = 2  --> 合法，隐式指定类型
var a : Int  --> 合法，显示指定类型

常量的初值可以在声明时指定，也可以在第一次使用时指定。

**划重点：**
由于Kotlin程序编译的字节码必须遵循JVM规范，因此对于直接在Kotlin程序中定义的变量、函数（如下面声明的变量a），kotlinc会自动生成一个名为“文件名首字母大写 + Kt”的类，并将变量转换为该类的静态的getter, setter方法（val只有getter），函数则转换为该类的静态方法，同时要求不能在该包下重复定义同名的类。
如我们定义了一个名为peng.kt的Kotlin文件，且该文件中包含了函数或变量/常量，那么kotlinc就会自动生成PengKt类，因此我们不能在包下重复定义PengKt类

```kotlin
var a : Int		
```

## 2.3 整型

Kotlin提供了4种整型：Byte, Short, Int, Long（所占位数及表示范围和Java相同，并兼容Java中对应的包装类型和其基本数据类型）
注意；
Kotlin的整型不是基本类型，而是引用类型！
Kotlin是null安全的语言，因此上述类型不能接受null值，要存储null值应使用Byte?, Short?, Int?, Long?类型。
由此观之，Kotlin允许在已有的数据类型后添加"?", 添加"?"之后的数据类型相当于对原类型进行了扩展，可支持被赋予null值
Kotlin中的普通类型的整型变量（如Int)会被映射成Java的基本类型（如int），带有"?"的整型变量会被映射成相应基本类型的包装类。

##### 字符串模板：

在字符串中嵌入${}， ${}内可放入变量或表达式，Kotlin会自动将该变量或表达式的值“嵌入”字符串内。如：
var str1 = "Peng"
var str2 = "${str1} is learning Kotlin !"
str2等价于： "Peng is learning Kotlin !"

为了提高数值的可读性，Kotlin允许为数值（包括浮点型）则呢更加下划线作为分隔符，也可以在数值前添加额外的零。如：
var num = 245_535_54  -->  var num = 24553354

## 2.4 浮点型

Float, Double  使用同整型一样
Kotlin还提供了：正无穷大（正浮点数除以0.0）、 负无穷大（负浮点数除以0.0）、 非数（0.0除以0.0，或对负数开方） 这3个特殊的浮点型数值

## 2.5 字符型

用于表示单个字符，字符型值必须用单引号(')括起来。
与Java不同的是，Kotlin的Char变量不能当成整数值使用。即Char型变量或表达式不能赋值给整型变量，反之亦然。



## 2.6数值型之间的类型转换

#### 2.6.1 整型之间的转换

**与Java不同的是，Kotlin不支持范围小的数据类型隐式转换为范围大的类型。**

Kotlin要求不同整型变量或值之间必须进行显示转换，并提供了一下方法进行转换：

* toByte()
* toShort()
* toInt()
* toLong()
* toFloat()
* toDouble()
* toChar()

与Java相同的是，在Kotlin中把范围大的变量转换为范围小的类型时，可能会发生``溢出``

虽然Kotlin缺乏隐式转换，但是Kotlin在表达式中可以自动转换。

```kotlin
var a : Byte = 70
var b : Short = 330
var c = a + b  //算数表达式中的a和b会自动提升为Int
var a = a.toLong() + b.toByte()  //表达式中最高级的操作数为Long, 因此整个表达式类型为Long
```

Char类型的值虽然不能被当成整数进行算数运算，但是Kotlin为Char类型提供了加、减运算支持。计算规则如下：

* **Char型值加、减一个整型值：**Kotlin会先将该Char型值对应的字符编码进行加、减该整数，然后将计算结果转换为Char型值
* **两个Char型值进行相减：** 将两个Char型值的字符编码进行减法运算，然后返回结果的Int型值。``注意：两个Char型值不能相加``

``` kotlin
var ch1 = 'i'
var ch2 = 'k'
println(c1 + 4)  //输出 m
println(c1 - 4)  //输出 e
println(c1 - c2)  //输出 -2
```



#### 2.6.2 浮点型与整型之间的转换

浮点型数据转换成整型数据时，小数部分会被截断。如：4.74会变成4，-2.5会被截断成-2

**当进行数据类型转换时，应尽量向表示范围大的数据类型转换，这样程序会更安全。**

Kotlin各种数据类型的表示范围：Byte（8位） -> Short （16位）-> Int（32位） -> Long（64位） -> Float（32位） -> Double（64位）



#### 2.6.3 表达式类型的自动提升

当一个算数表达式中包含多个数值型的值时，整个算数表达式的数据类型将发生自动提升。规则如下：

* 所有Byte，Short类型将被提升到 Int型
* 整个算数表达式的数据类型自动提升到与表达式中最高等级操作数相同的类型。操作数的等级排列如右：Byte -> Short -> Int -> Long  -> Float -> Double

``` kotlin
var vaule : Short = 5  //定义一个Short变量
vaule = value - 2  //右边的表达式类型将被自动提升为Int, 而左边数据类型为Short，发生错误！
```

当表达式中包含字符串时，是一种比较特殊的情况。

* 当把加号(+)放在字符串和数值/字符之间时，加号是一个连接运算符：String + Int  -->  字符串拼接（原字符串+ ‘该数值’）； String + Char  -->  字符串拼接。
* 当把加号放在字符和数值之间时，则是做加法运算（该字符的字符编码值 + 整型数值， 结果为数值）

``` kotlin
println("Hello!" + 'a' + 7)  //输出：Hello!a7
println('a' + 7 + "Hello!")  //输出：hHello!
```

## 2.7 Boolean型

Boolean型，用于表示逻辑上的“真”或“假”。与Java类似，Kotlin的Boolean型的值只能是true或false,不能用0或者非0来代表。

Boolean型变量同样可以“插值”到字符串中：

``` kotlin
var b = true;
var str1 = "${b}代表真"
var str2 = b + "代表真"
println(str1)  //输出：true代表真
println(str2)  //输出：true代表真
```

Boolean类型主要用于流程控制，如用于：if条件控制语句、while循环控制语句、do while循环控制语句。

## 2.8 null安全

null安全可以说是Kotlin相对于Java的重大改进之一，有效地避免了空指针异常（NullPointerException，即NPE）。

#### 2.8.1 非空类型和可空类型

``` kotlin
var str = "fkit"
var num1: Int = str.toIntOrNull()  //编译不通过：表达式右边可能为null，而左边的数据类型不支持null值
var num2: Int? = str.toIntOrNull()  //编译通过：左边的数据类型支持null值
```

由以上代码可知，Int?是可空类型，Int是非空类型。同理，String?, Char?, Long?, Double?...等是可空类型，String, Char, Long, Double...等是非空类型。

为了避免NPE，Kotlin对可空类型进行了限制：如果不加任何处理，可空类型不允许直接调用方法、访问属性。

``` kotlin
var str: String? = "hello world"
println(str.length)  //编译不通过：不允许直接调用可空类型的方法或属性
```

#### 2.8.2 先判断，后使用

对于可控类型变量，应先判断该变量不为null，然后在调用该变量的方法或属性。

``` kotlin
var str: String? = "hello world"
if(str != null){
    println(str.length)  //编译通过
}
```

在这里有一点需要提一下：**Kotlin的if条件必须是Boolean类型！（Boolean和Boolean?本质上是两种类型！）**

``` kotlin
var str: String? = "hello world"
var flag: Boolean? = (str != null)
if(flag){  //编译不通过：if条件必须是Boolean而不能是Boolean?
    println(str.length)
}
```

#### 2.8.3 安全调用

安全调用在Java中已经出现很多年了，只不过没有出现在Java的官方语法中。

``` java
user?.dog?.name
```

上述代码表示：如果user不为null，则返回user的dog属性；如果dog属性值不为null，则继续获取dog属性值的name属性值。反之，只要user为null，或者user.dog为null，那么该表达式不会导致NPE，而是整个表达式返回null——这就是Java中的**安全调用**。

Kotlin中的安全调用也与此类似：

```kotlin
var str: String? = "hello world"
println(str?.length)
```

Kotlin的安全调用也支持链式调用：

``` kotlin
user?.dog?.name
```

#### 2.8.4 Elvis运算

Elvis运算是一种小技巧，其实就是if else的简化写法。

```kotlin
var str: String? = "hello world"
var len1 = if(str != null) b.length else -1
var len2 = str?.length ?: -1  //Elvis运算符
```

Elvis运算符的含义是：如果“?:”左边的表达式不为null，则返回左边表达式的值；否则返回”?:”右边表达式的值。

#### 2.8.5 强制调用

在Kotlin中可以通过“!!.”强制调用可空变量的方法或属性，但这样可能会导致NPE.

``` kotlin
var str: String? = "hello world"
println(str!!.length)  //编译通过
```



## 2.9 字符串

#### 2.9.1 字符串类型

即String类型。与Java不同的是，Kotlin中String允许通过s[i]的格式（即下标索引）来访问指定索引处的字符。

``` kotlin
var str = "hello world"
println(str[2])
```

Kotlin中的字符串有两种字面值：

1. 转义字符串：类似于Java中的字符串。
2. 原始字符串：可以包含换行和任意文本。需要用3个双引号括起来。

``` kotlin
var str1 = "hello world"  //定义普通字符串（即转义字符串）
var str2 = """
	|天上白玉京，
	|十二楼五城。
	|仙人扶我顶，
	|结发受长生。
""".trimMargin()  //定义原始字符串
```

上述代码中的str2即为定义的原始字符串。trimMargin()方法可以去掉原始字符串前的缩进，默认情况下，Kotlin使用竖线（|）作为边界符。即所有竖线之前的内容都会被去掉。开发者也可以使用其他字符作为边界符，只需在trimMargin()方法中传入该边界符作为参数即可。

#### 2.9.2 字符串模板

Kotlin允许在字符串中嵌入变量或表达式，只要将变量或表达式放入${}中即可。

#### 2.9.3 Kotlin字符串的方法

Kotlin的String和Java的String不是同一个类，他们的方法略有不同。Kotlin的String类提供了更多的方法。如：

* toXxx()：将字符串转换成数值。
* capitalize()：首字母大写
* decapitalize()：首字母小写
* commonPrefixWith()：返回两个字符串相同的前缀
* commonSuffixWith()：返回两个字符串相同的后缀
* ......

## 2.10 类型别名

Kotlin提供了类似于C语言中typedef的功能：可以为已有的类型指定一个可读性更强的名字。Kotlin提供了**typealias**来定义类型别名。其语法格式为：

`` typealias 类型别名 = 已有类型``





















