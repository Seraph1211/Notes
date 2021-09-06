# 流程控制

与Java类似，Kotlin同样提供了两种基本的流程控制结构：**分支结构**和**循环结构**。

Kotlin提供了 if 和 when 两种分支语句，其中 when 语句可以代替Java的switch语句，且功能更强大；Kotlin提供了 while 、do while 、for-in 循环，抛弃了Java原有的普通for循环。Kotlin也提供了 break 和 continue 来控制循环结构。



## 4.1 分支结构

### 4.2.1  if 分支

Kotlin中的 if 分支既可以作为语句使用，也可以作为表达式使用。

``` kotlin
//作为语句使用的三种形式：
//形式1
if (expression) {
    statements...
}

//形式2
if (expression) {
    statements...
} else {
    statements...
}

//形式3
if (expression) {
    statements...
} else if (expression) {
    statements...
}
...
// 注意：使用if else语句时，一定要先处理范围更小的情况

```

### 4.2.2  if 表达式

``` kotlin
// if 作为表达式使用时：
var age = 20
//将if表达式赋值给str
var str = if (age > 20) "age大于20" else if (age < 20) "age小于20" 
		else "age等于20"   
//由上述代码可知，当if分支作为表达式使用时，整个if表达式最终会返回一个值，因此我们可以
//用它来代替Java中的三目运算符。
//注意：if表达式必须要有返回值
```



### 4.2.3 when分支语句

when分支取代了Java中原有的switch语句。

``` kotlin
var score = 'B'
when (score) {
    'A' -> {
        println("You get a A !")
    }
    'B' -> {
        println("You get a B !")
    }
    'C' -> {
        println("You get a C !")
    }
    'D' -> {
        println("You get a D !")
    }
    else -> {
     	println("Flunk !")
    }
}
```

when分支相对于switch分支的改进：

1. when分支可以匹配多个值。
2. when分支后的值不要求是常量，可以是任意表达式。
3. when分支对条件表达式的类型没有任何要求。

``` kotlin
// when分支可以匹配多个值
var score = 'B'
when (score) {
    'A', 'B' -> {
        println("优秀")
    }
    'C', 'D' -> {
        println("良好")
    }
}
```

``` kotlin
// when分支后的值不要求是常量，可以是任意表达式
var score = 'B'
var str = "EFGH"
when (score) {
    str[0] - 4, str[1] - 4 -> {
        println("优秀")
    }
    str[2] - 4, str[3] - 4 -> println("良好")
}
```

``` kotlin
// when分支的条件表达式可以是任意类型
var date = Date()
when (date) {  // 以Date类型作为when的条件表达式
    Date() -> {
        println("666")
    }
    else -> {
        println("555")
    }
}
```

### 4.2.4  when 表达式

与 if 分支相同，when分支也可以作为表达式使用。when表达式必须有返回值，因此when表达式必须有else分支。

``` kotlin
var score = 'B'
val str = when (score) {
    'A' -> {
        println("666")
        "优秀"
    }
    'B' -> {
        println("it's ok")
        "良好"
    }
    else -> {
        println("555")
        "不及格"
    }
}
println(str)  //输出：良好
```

### 4.2.5  when 分支处理范围

通过使用 in 、 !in 运算符，我们还可以使用when分支检查表达式是否位于指定区间或集合中。

``` kotlin
val age = java.util.Random().nextInt(100)
//通过when表达式对str赋值
var str = when (age) {
    in 10..25 -> "年轻人"
    in 25..50 -> "中年人"
    in 51..80 -> "老年人"
    else -> "牛人"
}
```

### 4.2.6 when 分支处理类型

通过使用 is 、!is 运算符，我们可以使用when分支检查表达式是否为指定类型。

``` kotlin
var element: String = "hello world"
when (element) {
    is String -> println("String类型")
    is Int -> println("Int类型")
    is Double -> println("Double类型")
    else -> println("其他类型")
}
```

### 4.2.7 when条件分支

我们可以用when分支来代替 if..else if 链，具体代码不做赘述。



## 4.3  循环结构

### 4.3.1  while循环

与Java相同，不做赘述。

### 4.3.2 do while 循环

同上。

### 4.3.3  for - in 循环

for - in 循环格式如下：

``` kotlin
for (常量名 in 字符串 | 范围 | 集合) {
    statements...
}
```

上述语法格式有两点需要注意：

1. ``for - in 循环中的常量无需声明``。 for - in 循环中的常量会在每次循环开始前自动被赋值，因此该常量无需提前声明。
2. `` for - in 循环可以遍历任何可迭代对象``。所谓的可迭代对象就是该对象包含一个iterator()方法，且该方法的返回值对象具有next()、hasNext()方法。

``` kotlin
//for-in循环的简单使用
for (num in 2..5) {
    println(num)
}
```

注意：for - in 循环中的循环计数相当于一个用val声明的常量，因此程序不允许在for - in循环中对循环计数器进行赋值。

``` kotlin
for (num in 2..5) {
    println(num)
   	num = 8;  //对循环计数器num进行赋值会报错
}
```

### 4.3.4 嵌套循环

与Java相同，不做赘述。



## 4.4 循环控制结构

Kotlin提供了 ``continue`` 和 ``break`` 来控制循环结构，除此之外还可以使用 `` return`` 来结束整个方法。

### 4.4.1 使用 break 结束循环

Kotlin中的break语句基本与Java类型，不过有一点值得一提：在Kotlin中，break语句不仅可以结束其所在的循环，还可以直接结束其外层循环。此时需要在break后紧跟一个标签，用于标识一个外层循环。

Kotlin中的标签就是一个紧跟着@的标识符。Kotlin中的标签只有放在循环语句之前才起作用。

``` kotlin
//外层循环，用outer标识
outer@ for (i in 0 until 5) {
    //内层循环
    for (j in 0 until 3) {
        println("i = ${i}, j = ${j}")
        if (j == 2) {
           break@outer  //该break不是结束其所在的循环，而是结束outer所标识的循环
        }
    }
}
```

### 4.4.2 使用 continue 忽略本次循环剩下的语句

使用同break语句，不做赘述。

### 4.4.3 使用return结束方法

同Java，不做赘述。







