# 数组和集合

Kotlin为数组增加了一个Array类，为元素是基本类型的数组增加了XxxArray类（其中Xxx可以是Byte、Short、Int等基本类型），因此开发者可以用面向对象的语法来使用Kotlin的数组，包括创建数组对象、调用数组对象的属性和方法等。

Kotlin的集合体系抛弃了Java中的Queue集合，但增加了``可变集合``和``不可变集合``的概念。Kotlin的集合体系由三种集合组成：List、Set、Map

![image-20210422172932678](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210422172932678.png)

上述三种集合性质与Java类似。

## 5.1  数组

Kotlin的数组使用Array<T>类代表，即Kotlin的数组就是一个Array类的实例，因此Kotlin数组也算引用类型。

### 5.1.1  数组的创建

Kotlin创建数组即创建Array<T>类的实例，大致有如下两种方式：

* 通过Array<T>类的构造器来创建实例：Array(size: Int, init: (Int) -> T) 构造器
* 通过工具函数创建实例：arrayOf()、arrayOfNulls()、emptyArray()

``` kotlin
   var arr1 = arrayOf("Java", "Kotlin", "C", "C++")  //创建包含指定元素的数组

    var arr2 = arrayOfNulls<Int>(5)  //创建指定长度、指定类型、元素为null的数组

    var arr3 = emptyArray<Double>()  //创建长度为0的空数组

    var arr4 = Array(5, {(it*2 + 97).toChar()})  //使用Array<T>类的构造器来创建数组，并用Lambda表达式初始化数组元素
    
```

Array<T>类要求它的元素必须是引用类型（基本类型的值放入会自动装箱）。

Kotlin提供了ByteArray、ShortArray、IntArray、LongArray......用于映射Java的byte[]、short[]、int[]、long[]......

``` kotlin
var intArr = intArrayOf(2, 5, 7, 8)
var stringArr = stringArrayOf("kotlin", "java")
```

### 5.1.2  使用数组

数组的使用一般就是对数组元素的访问。访问数组元素通常是通过"数组引用+[]"实现的，而Kotlin中'[]'实际是get和set方法实现的。所以在Kotliin中既可以用'[]'符号访问数组元素，也可用get / set 方法访问数组元素。

``` kotlin
var strArr = arrayOf("Kotlin", "Java")
println(strArr[0])
println(strArr.get(1))
strArr[0] = "null"
strArr.set(1, "null")

println(str.size)  //输出数组长度
```

### 5.1.3  使用 for-in 循环遍历数组

Kotlin的 for-in 循环可以自动遍历数组中的每个元素，但是使用for-in循环遍历数组时不允许对循环变量进行赋值！（之前已经提到过，循环变量是val型）

``` kotlin
var languages = arrayOf("Java", "Kotlin", "C", "C++", "C#")
for(language in languages) {
    println(language)
}
```

### 5.1.4  使用数组索引

Kotlin的数组提供了一个indices属性，该属性可返回数组的索引区间。

``` kotlin
var languages = arrayOf("Java", "Kotlin", "C", "C++", "C#")
for(i in languages.indices) {   //等效于 i in 0 until languages.size
    println(languages[i])
}
//通过索引区间遍历的实现具有更好的性能

val last = languages.lastIndex  //Kotlin提供了lastIndex属性，返回数组最后一个元素的索引值

//通过 withIndex() 可以同时访问数组的索引和元素
for((index, value) in languages.withindex()) {
    println("索引为${index}的元素是${value}")
}
```

### 5.1.5 数组常用的方法

* asList()：将数组转换成List集合
* distinct()：去掉数组中的重复元素
* drop | dropLast(n: Int)：去掉数组前面或后面n个元素
* indexOf | lastIndexOf(element: T)：获取从前搜索或从后搜索时元素element在数组中的位置
* max | min()：按照自然序排序规则找出数组中最大或最小的元素
* ......

### 5.1.6  多维数组

将一维数组中的元素定义为数组即使多维数组。

``` kotlin
var a = arrayOfNulls<Array<Int>>(4)
a[0] = arrayOf(2, 5)
a[0]?.set(0, 9)
```



## 5.2 Kotlin集合概述

Kotlin中的集合类同样由Collection和Map这两个接口派生。

Kotlin只提供了HashSet、HashMap、LinkedHashSet、LinkedHashMap、ArrayList 这5个集合实现类，且都是可变集合。

我们只能通过函数来创建不可变集合。



## 5.3 Set集合

Kotlin中的Set集合与Java基本相同。

### 5.3.1  创建Set集合

不推荐使用构造器创建Set集合，推荐使用函数来创建Set集合：

* setOf()：返回不可变的Set集合。该函数可接受0个或多个参数，这些参数将作为集合的元素。
* mutableSetOf()：返回可变的MutableSet集合。该函数可接受0个或多个参数，这些参数将作为集合的元素。
* hashSetOf()：返回可变的HashSet集合。该函数可接受0个或多个参数，这些参数将作为集合的元素。
* linkedSetOf()：返回可变的LinkedHashSet集合。该函数可接受0个或多个参数，这些参数将作为集合的元素。
* sortedSetOf()：返回可变的TreeSet集合。该函数可接受0个或多个参数，这些参数将作为集合的元素。

``` kotlin
var set = setOf("Java", "Kotlin")
var mutableSet = mutableSetOf("Java", "Kotlin")
```

使用setOf()、mutableSetOf()、linkedSetOf()创建的集合能维护元素的添加顺序，sortedSetOf()创建的集合维护元素的大小顺序。

### 5.3.2  使用Set的方法

除Java中Set集合所提供的方法外，Kotlin还扩展了一些方法，在此不做赘述。



## 5.4 List集合

Kotlin中的 List 集合与Java基本相同。

### 5.4.1 声明和创建 List 集合

同Set，不推荐用构造器来创建List 集合，推荐使用工具函数创建List 集合。（Kotlin并未真正实现List 集合，而是通过别名借用了Java中的ArraylList集合）。

* listOf()：返回不可变的List 集合，可接受0个或多个参数并作为集合元素。
* listOfNotNull()：返回不可变的List 集合，会自动去掉传入参数中的null值，即返回一个不含null的List 集合
* mutableListOf()：返回可变的MutableList集合，可接受0个或多个参数并作为集合元素。
* arrayListOf()：返回可变的ArrayList集合。可接受0个或多个参数并作为集合元素。

### 5.4.2  使用List的方法

* get()
* indexOf()：返回元素在集合中的索引。
* lastIndexOf：返回List集合的子集合。
* subList()：返回List集合的子集合。
* ......

### 5.4.3 可变List

使用函数mutableListOf()、arrayListOf()创建的List是可变的，可以对其执行添加、插入、删除、替换等操作。



## 5.5 Map集合

Kotlin中的 Map 集合与Java基本相同。

### 5.5.1  声明和创建Map集合

同Set和List ，推荐使用工具函数创建Map集合。

* mapOf()：返回不可变的Map集合，可接受0个或多个key-value对并将其作为集合元素。
* mutableMapOf()：返回可变的MutableMap集合，可接受0个或多个key-value对并将其作为集合元素。
* hashMapOf()：返回可变的HashMap集合，可接受0个或多个key-value对并将其作为集合元素。
* linkedMapOf()：返回可变的LinkedHashMap集合，可接受0个或多个key-value对并将其作为集合元素。
* sortedMapOf()：返回可变的TreeMap集合，可接受0个或多个key-value对并将其作为集合元素。

### 5.5.2  使用Map的方法

略

### 5.5.3  遍历Map

* 通过"[]"运算符根据key来获取value从而实现遍历
* for-in 循环





