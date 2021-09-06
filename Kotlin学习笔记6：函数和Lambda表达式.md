# 函数和Lambda表达式

Kotlin融合了面向过程语言和面向对象语言的特征，相比于Java，它增加了对函数式编程的支持，支持定义函数、调用函数。相比于C语言，Kotlin支持局部函数（Lambda表达式的基础）。

## 6.1  函数入门

### 6.1.1  定义和调用函数

定义函数的语法格式如下：

``` kotlin
fun 函数名 (形参列表) [: 返回值类型] {
	//函数体
}
// 函数的声明必须使用fun关键字

// 形参列表   “形参名: 参数类型”

// 举例：
fun max(x: Int, y: Int): Int {
    val res = if(x > y) x else y
    return res
}
```

### 6.1.2  函数返回值和Unit

如果希望一个函数没有返回值，可以通过以下两种方式实现：

1. 省略“: 返回值类型”部分
2. 使用“: Unit”指定返回Unit -> 代表没有返回值

``` kotlin
fun func1(str: String) {
	println(str)
}

fun func2(str: String): Unit {
    println(str)
}
```

### 6.1.3  递归函数

在函数体内调用该函数本身。

......

### 6.1.4  单表达式函数

如果函数只是返回单个表达式，那么可以省略花括号并在函数名后用等号指定函数体。这种形式的函数被称为单表达式函数。

``` kotlin
fun area1(x: Int, y: Int): Int {
    return x*y;
}

fun area2(x: Int, y: Int): Int = x*y

//函数area1和函数area2等效，其中area2是单表达式函数
```

## 6.2  函数的形参

相比于Java，Kotlin形参的功能更加丰富且灵活。

### 6.2.1  命名参数

Kotlin中参数名是有意义的。

Kotlin除了可以按照传统的方式——根据参数的位置（顺序）来传入参数之外，还可以使用命名参数来传参(此时可以不按顺序传参）。

还可以以上二者混合使用，但此时要求位置参数必须位于命名参数之前。

``` kotlin
fun area(width: Double, height: Double): Double {
    var a = width * height
    println("width=${width}, height=${height}, area=${a}")
    return a
}

var res: Double

//传统的传参方式——位置参数，根据形参位置传参
res = area(2.0, 5.0)  

//Kotlin中支持的传参方式——命名参数，根据形参名传参
res = area(heigth = 5.0, width = 2.0)

//二者的混合使用
res = area(2.0, height = 5.0)
```

### 6.2.2  形参默认值

在Kotlin中，用户可以在定义函数时为一个或多个形参指定默认值——这样在调用函数时，就可以省略该形参值的传入，而使用形参默认值。语法格式如下：

`` 形参名: 形参类型 = 默认值``

```kotlin
fun sayHi(name: String = "Seraph", msg: String = "hello !") {
    println("${name}, ${msg}")
}

sayHi()  //全部使用默认参数，输出：Seraph, hello !
sayHi("Jack", "see you !")  //全部使用位置参数
sayHi(name = "Leo", msg = "bye~") //全部使用命名参数
sayHi(msg = "bye~")  //默认参数 + 命名参数
sayHi("Ben")  // 位置参数 + 默认参数， 输出：Ben, hello !
```

通常建议将带默认值的参数定义在形参列表的最后。

### 6.2.3  尾递归函数

当函数将自身作为其执行体的最后一行代码，且递归调用后没有更多的代码时，可以使用尾递归语法。

尾递归不能在try、catch、finally块中使用。

尾递归函数需要使用tailrec修饰。

``` kotlin
//定义计算阶乘的函数
fun fact(n: Int): Int {
    if(n == 1) {
        return 1
    } else {
        return n * fact(n-1)
    }
}

//使用尾递归语法改写上述函数
tailrec fun factRec(n: Int, total: Int = 1): Int = if(n == 1) total else factRec(n-1, total * n)
```

编译器会将尾递归优化成一个快速且高效的基于循环的版本，这样可以减少对内存的消耗。

### 6.2.4  个数可变的形参

Kotlin中，我们可以通过在形参名前添加`` vararg``修饰符，以定义个数可变的形参，从而为函数指定数量不定的形参。

``` kotlin
fun test(a: Int, vararg books: String) {
    //books被当做数组处理
    for(book in books) {
        println(book)
    }
}
```

Kotlin允许个数可变的形参位于参数列表中的任意位置，但是要求一个函数只能带一个形参。



## 6.3  函数重载

一个kt文件中包含了两个或两个以上函数名相同、形参列表不同的函数，被称为函数的重载。

与Java类似的是，Kotlin中函数的重载也只能通过形参列表进行区分，形参个数不同、形参类型不同都可以算作函数的重载。但是形参名不同、返回值类型不同或修饰符不同，都不能算函数的重载。



## 6.4  局部函数

Kotlin支持在函数体内定义函数，这种定义在函数体内的函数别称之为局部函数。

默认情况下，局部函数是对外部隐藏的，只在其封闭函数内有效。其封闭函数也可以返回局部函数，以便程序在其他作用域中使用局部函数。

``` kotlin
fun A(type: String) {
    fun B(name: String) {
        println("name=${name}")
    }
}
```



## 6.5  高阶函数

Kotlin中函数也是一等公民，函数本身也有自己的类型——函数类型。函数类型既可以用于定义变量，也可以用于形参、返回值。

### 6.5.1  使用函数类型

函数类型由：函数形参列表、-> 、返回值类型 这三者组成。

``` kotlin
fun foo(a: Int, name: String) -> String {
    ......
}
// 函数foo的函数类型为：(Int, String) -> String

fun bar() {
    ......
}
// 函数bar的函数类型为：() 或 () -> Unit
```

使用函数类型定义变量的方法如下：

``` kotlin
var myFun: (Int, Int) -> Int

fun pow(base: Int, exponent: Int): Int {
    var res = 1
    for(i in 1 .. exponent) {
        res *= base
    }
    return res
}

fun area(width: Int, height: Int): Int = width * height

myFun = ::pow  //将pow函数赋值给myFun
println(myFun(2, 3))  //输出：8

myFun = ::pow  //将area函数赋给myFun
println(myFun(2, 3))  //输出：6
```

当直接访问一个函数的函数引用，而不是调用该函数时，需要在函数名前添加两个冒号，而且不能在函数名后添加圆括号——一旦添加圆括号就变成了调用函数而不是访问函数引用。

### 6.5.2  使用函数类型作为形参

``` kotlin
fun map(data: Int, fn: (Int) -> Int): Unit {
    ......
}
```

### 6.5.3  使用函数类型作为返回值类型

``` kotlin
fun getMathFunc(type: String): (Int) -> Int {
    
    fun square(n: Int): Int = n * n
    
    fun cube(n: Int): Int = n * n * n
    
    fun factorial(n: Int): Int {
        var res = 1;
        for(index in 2 .. n) {
            res *= index
        }
        return res
    }
    
    when(type) {
        "square" -> return ::square
        "cube" -> return ::cube
        else -> return ::factorial
    }
}
```



## 6.6  局部函数与Lambda表达式

Lambda表达式是现代编程语言争相引入的语法，它是功能更灵活的代码块，可以在程序中被传递和调用。

### 6.6.1  回顾局部函数

略

### 6.6.2  使用Lambda表达式代替局部函数

可以使用Lambda表达式来简化局部函数。

``` kotlin
fun getMathFunc(type: String): (Int) -> Int {
    when(type) {
        "square" -> return {n: Int -> 
                           	 n * n
                           }
        "cube" -> return {n: Int -> 
                         	n * n * n
                         }
        else -> return { n: Int -> 
                        	var res = 1
                        	for(index in 2 .. n) {
                                res *= index
                            }
                        	res
                       }
    }
}
```

Lambda表达式和局部函数区别如下：

* Lambda表达式总是被大括号括着
* 定义Lambda表达式不需要fun关键字，无需指定函数名
* 形参列表（如果有）在 -> 之前声明，参数类型可以省略。
* 函数体（Lambda表达式的执行体）放在 -> 之后
* 函数的最后一个表达式自动被作为Lambda表达式的返回值，无需使用 return 关键字

### 6.6.3  Lambda表达式的脱离

作为参数传入的Lambda表达式可以脱离函数独立使用。



## 6.7  Lambda表达式

Lambda表达式语法如下：

``` kotlin
{ (形参列表) -> 
	零到多条可执行语句	
}
```

### 6.7.1  调用Lambda表达式

Lambda表达式可以被赋值给变量或者直接调用。

``` kotlin
// 定义一个Lambda表达式，并将它赋值给square
var square = { n: Int ->
    n * n
}
println(square(5))  //输出：25

// 定义一个Lambda表达式，并在其后添加圆括号来调用该表达式，并将结果赋给res
var res = { base: Int, exponent: Int ->
    var result = 1
    for(i in 1 .. exponent) {
        result *= base
    }
    result
}(4, 3)
println(res)  //输出64
```

### 6.7.2  利用上下文推断类型

如果Kotlin根据Lambda表达式的上下文推断出形参类型，那么Lambda表达式就可以省略形参类型。

``` kotlin
// 变量square的类型已经被声明了，因此Kotlin可以推断出Lambda表达式的形参类型
// 所以Lambda表达式可以省略形参类型
var square: (Int) -> Int = {n -> n * n}

var res = {a, b -> a + b}   //非法，因为Kotlin无法推断形参a,b的数据类型
```

### 6.7.3  省略形参名

如果**只有一个形参**，那么Kotlin允许省略Lambda表达式的形参名。当形参名被省略时，Lambda表达式用 `` it``来代表形参。同时， -> 也不需要了。

``` kotlin
//  省略形参名，用it代替
var square: (Int) -> Int = {it * it}
```

### 6.7.4  调用Lambda表达式的约定

Kotlin中约定：如果函数的最后一个形参是函数类型，且你打算传入一个Lambda表达式作为相应的参数，那么就允许在圆括号之外指定Lambda表达式。

``` kotlin
var list = listOf("Java", "Kotlin", "Go")
var rt = list.dropWhile(){it.length > 3}   //dropWhile()方法的参数列表为： (T) -> Boolean
println(rt)  //输出: [Go]
```

如果Lambda表达式是函数调用的唯一参数，则调用时函数的圆括号可以省略：

``` kotlin
var rt = list.dropWhile{it.length > 3}
```

### 6.7.5  个数可变的参数和Lambda参数

前面有提到：个数可变的形参可以定义在参数列表中的任意位置，但是如果不将它放在最后，就只能用命名参数的形式为可变形参之后的其他形参传值。所以建议将可变形参放在形参列表的最后。

那么我们到底是应该将函数类型的参数放在形参列表的最后，还是将个数可变的参数放在形参列表最后呢？

答案：如果一个函数既包含个数可变的形参，也包含函数类型的形参，那么应该将函数类型的形参放在最后。

``` kotlin
fun <T> test(vararg name: String, transform: (String) -> T): List<T> {
    ......
}
```



## 6.8 匿名函数

Lambda表达式无法指定返回值类型，且在一些特殊场景下Kotlin也无法推断出Lambda表达式的返回值类型。此时可以匿名函数来代替Lambda表达式。

只要将普通函数的函数名去掉就成了匿名函数。

``` kotlin
var test = fun(x: Int, y: Int): Int {
    return x + y
}
```



## 6.9  捕获上下文中的变量和常量

Lambda表达式、匿名函数、局部函数都可访问或修改其所在上下文中的变量和常量。



## 6.10  内联函数

由于函数的调用过程会产生一定的时间和空间上的开销，为了避免这部分开销（即避免产生函数的调用过程），我们可以考虑通过``内联函数`` 的方式，将被调用的函数或表达式”嵌入“原来的执行流中。

```kotlin
// 通过inline关键字声明内联函数
inline fun mFun(data: Int) {
    ......
}
```





