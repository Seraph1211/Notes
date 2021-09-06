# 运算符和表达式

Kotlin提供了一系列功能丰富的运算符，包括算数运算符、比较运算符、逻辑运算符、区间运算符、位运算符等。Kotlin基本支持Java的全部运算符（Kotlin不支持三目运算符，但是可以用if表达式代替三目运算符）。

## 3.1 与Java相同的运算符

Kotlin不支持三目运算符，且位运算符与Java也略有区别。除此之外，Java支持的运算符Kotlin也基本支持。

注意：Kotlin的运算符都是以方法的形式来实现的。各种运算符对应的方法名都是固定的，我们只要为某类型提供特定名称的方法（成员方法或扩展方法都行），那么接下来我们就可以对该类型的对象使用相应的运算符（如我们为A类提供了plus方法，那么我们就可以对A类的对象使用"+”运算符）。由此观之，Kotlin所有的运算符的功能都是广义的，不仅能作用于数值型、字符串，还可以作用于任意自定义的Kotlin类。**即Kotlin支持运算符的重载。**

#### 3.1.1 单目前缀运算符

单目前缀运算符有：+, -, !

![image-20210418103759629](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418103759629.png)

``` kotlin
var a = 20
    val b = -a
    val c = a.unaryMinus()

    println("b=${b}, c=${c}")  //输出：b=-20, c=-20
//由此可见，-a和a.unAryMinus()效果一样。
```

#### 3.1.2 自加和自减运算符

自加：++  

自减：--

![image-20210418104354652](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418104354652.png)

* 当++、--放在变量前时（++a, --a）：
  1. 先对变量调用inc()、dec()方法，并将方法返回值赋给变量。
  2. 自加、自减表达式返回变量的新值。
* 当++、--放在变量后时（a++, a--）：
  1. 先用一个临时变量缓存变量的值。
  2. 对变量调用inc()、dec()方法，并将方法返回值赋给变量。
  3. 自加、自减表达式返回临时变量的值。

#### 3.1.3 双目算数运算符

![image-20210418105028478](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418105028478.png)

![image-20210418105038556](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418105038556.png)

#### 3.1.4 in 和 !in 运算符

in 和 !in是Kotlin中的一个语法糖。

* a in b ：判断a是否包含于b
* a !in b ：判断a是否不包含于b

![image-20210418105204905](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418105204905.png)

~~~ kotlin
var strP = "hello world"
var strS = "hello"
println(strS in strP)  //输出：true
val array = arrayOf(24, 45, 89, -2)
val a = 2
println(a !in array)  //输出：true 
~~~

#### 3.1.5 索引访问运算符

![image-20210418114108555](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418114108555.png)

Kotlin中的String和ArrayList等都可以通过索引访问运算符获取指定索引处的元素，这也是Kotlin提供的语法糖。

#### 3.1.6 调用运算符

![image-20210418114351554](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418114351554.png)

``` kotlin
val s = "java.lang.String"
val mtd = Class.forName(s).getMethod("length")  //使用反射获取String类的length()方法
println(mtd.invoke("java"))  //传统方法：使用Method对象的invoke()方法，输出4
println(mtd("java"))  //使用调用运算符，输出4
```

由上述代码可知，调用运算符其实就是省略了invoke方法名。

#### 3.1.7 广义赋值运算符

![image-20210418114911276](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418114911276.png)

#### 3.1.8 相等与不等运算符

![image-20210418115244477](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418115244477.png)

由上表可知，Kotlin中的"=="与Java不同，它不再是比较两个变量是否引用在同一个对象上（即比较的不再是地址），而是与Java中的equals()基本等义（只不过"=="空指针安全）。

**Java中提供的"==" 和 "!=" 在Kotlin中则是由"===" 和 "!==" 代替了。**

#### 3.1.9 比较运算符

![image-20210418115816482](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418115816482.png)

由上表可知，比较运算符是由compareTo()方法实现的，因此原来Java中支持使用compareTo()方法比较大小的对象，都可以用比较运算符进行比较。

``` kotlin
var s1 = "java"
var s2 = "kotlin"
println(s1 > s2)  //输出：false
println(s1.compareTo(s2) > 0)  //输出：false
```

注意：String类比较大小的规则是按照字符的编号大小进行比较的——先比较两个字符串的首字母；如果首字母相同，在比较第二个字母......Java中也是这样。



## 3.2 位运算符

Kotlin中虽然也提供了与Java功能完全相同的位运算符，但是这些位运算符都不是以特殊字符给出的，而是以infix函数的形式给出，因此程序只能通过函数名来执行这些位运算符。

Kotlin支持的位运算符同样有如下7个：

1. and：按位与。同为1才返回1.
2. or：按位或。只要有一个为1就返回1.
3. inv：按位非。将操作数的每个位（包括符号位）全部取反。
4. xor：按位异或。参与比较的两位相同返回0，不同返回1.
5. shl：左移运算符。
6. shr：右移运算符。
7. ushr：无符号位右移。

**Kotlin的运算符只对 Int 和 Long 两种数据类型起作用。**

![image-20210418121018240](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210418121018240.png)

在进行移位运算时，只要被移位的二进制码没有发生有效位的数字丢失，左移n位就相当于乘以2的n次方，右移n位就相当于除以2的n次方。

在进行移位运算时，只是得到了一个新的运算结果，原来的操作数本身是不变的。



## 3.3 区间运算符

Kotlin提供了两种区间运算符：`` 闭区间运算符``、`` 半开区间运算符``， 它们可以非常方便地构建一种数据结构，以包含特定区间的所有值。

#### 3.3.1 闭区间运算符

闭区间运算符：``a..b``，用于定义区间 [a, b]. 要求 a 不大于 b.

如果a === b， 则会产生只有一个值（边界值）的区间。

``` kotlin
var range = 2..6
for(num in range){
    print("${num} ")
}
//输出：2 3 4 5 6
```

#### 3.3.2 半开区间运算符

半开区间运算符：`` a util b``，用于定义区间 [a, b). 要求 a 不大于 b.

如果a === b， 则会产生一个空区间（不包含任何值）。

``` kotlin
var nums = arrayOf(2, 5, 7, 3)
for(num in 0 until nums.size){  //用半开区间遍历数组等列表非常方便
    print("${num} ")
}
//输出：2 5 7 3
```

#### 3.3.3 反向区间

使用 `` downTo``运算符可以定义一个从大到小的闭区间。

``` kotlin
var range = 6 downTo 2
for(num in range){
    print("${num} ")
}
//输出：6 5 4 3 2
```

#### 3.3.4 区间步长

区间步长：区间内两个值之间的差值。

区间的默认步长为1，我们可以通过``step``运算符来显示地指定区间的步长。

``` kotlin
for(num in 7 downTo 1 step 2){
    print("${num} ")
}
//输出：7 5 3 1
```



## 3.4 运算符的重载

Kotlin的运算符都是靠特定名称的方法支撑的，因此只需要重载这些名称的方法，我们就可以为任意类添加这些运算符。

例如对单目前缀运算符进行重载，我们只需要为任意类定义名为unaryPlus()、unaryMinus()、not()方法，并以operator修饰该方法即可。

``` kotlin
data class Data(val x: Int, val y: Int){
    //以成员方法的形式为Data类定义unaryMinus()方法
    operator fun unaryMinus(): Data {
        return Data(-x, -y)
    }
}

//以扩展方法的形式为Data类定义not()方法
operator fun Data.not(): Data {
    return Data(-x, y)
}

fun main(args: Array<String>) {
    val d = Data(4, 10)
    println(-d)  //输出：Data(x=-4, y=-10)
    println(!d)  //输出：Data(x=-4, y=10)
}
```

其他运算符的重载同理。

