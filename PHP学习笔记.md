# 1、PHP语法

## 1.1  基本的PHP语法

PHP脚本以``<?php``开始，以`` ?>`` 结束，可以放在文档中的任何位置。

``` php+HTML
<?php
	//PHP代码
?>
```

PHP文件通常包含HTML标签和一些PHP脚本代码，其默认文件扩展名是".php".

PHP中每行代码都必须以分号结束。

如下是一个简单的PHP文件实例，它可以向浏览器输出文本"Hello World!":

``` php+HTML
<!DOCTYPE html>
<html>
	<body>
    	
        <h1>
            My first PHP page
        </h1>
    
        <?php
        	echo "Hello World!";
        ?>
    </body>
    
</html>
```

## 1.2  PHP中的注释

``` php+HTML
<!DOCTYPE html>
<html>
    <body>
        
        <?php
        	// 单行注释
        	
        	 /*
        	 * 多行注释
        	 */
        ?>
        
    </body>
    
</html>
```



# 2、PHP变量

## 2.1  PHP变量简介

PHP变量可以被赋予某个值或者表达式。

PHP变量规则：

* 变量以`` $``符号开始，后面紧跟变量名。
* 变量名必须以字母或者下划线开始。
* 变量名只能包含字母、数字和下划线，不能包含空格。
* 变量名区分大小写。

``` php
<?php
$x = 5;  //为PHP变量赋予某个值
$y = 6;
$z = $x + $y;  //为PHP变量赋予某个表达式
?>
```

## 2.2 创建PHP变量

PHP没有声明变量的命令，变量在第一次赋值时被创建。

PHP是一门弱类型语言，即我们在创建变量时不需要生命该变量的数据类型（在强类型语言中，我们必须在使用变量前声明变量的数据类型和名称），PHP会根据变量的值，自动把变量转换为正确的数据类型。

``` php
<?php
$str = "Hello World!";
$x = 4;
$y = 2.5;
?>
```

## 2.3  PHP变量的作用域

变量的作用域是脚本中变量可被引用/使用的部分。

PHP有四种不同的变量作用域：

* local
* global
* static
* parameter

### 2.3.1  局部和全局作用域

* 所有定义在函数外部的变量，拥有全局作用域。除了函数内部之外，全局变量可以被脚本中的任何部分访问。要在函数内部访问一个全局变量，必须使用``global``关键字。

* 在PHP函数内部声明的变量是局部变量，仅能在函数内部访问。

``` php
<?php
$x = 5;  // x为全局变量
function myTest() {
	$y = 10;  // y为局部变量
    global $x;  // 要在函数内部使用全局变量x, 必须使用global关键字
    $z = $x + $y;
}
?>
```

### 2.3.2  static作用域

当一个函数完成时，它的所有变量通常都会被删除。如果希望某个局部变量在函数结束后不被删除，可以使用``static`` 关键字来修饰它（该变量仍然是局部变量）。

然后，在每次调用该函数时，该变量将会保留着函数上一次调用结束时的值。

``` php
<?php
function fun() {
    static $x = 0;
    echo $x;
    x++;
    echo PHP_EOL;  //换行
}
	
fun();
fun();
fun();

/*
* 输出：
* 0
* 1
* 2 
*/

?>
```

### 2.3.3  参数作用域

参数是通过调用代码将值传递给函数的局部变量。

``` php
<?php
function fun($x) {
    echo $x;
}	
?>
```

# 3、echo 和 print

PHP中有两个基本的输出方式：`` echo`` 和 `` print``，它们之间的区别如下：

* echo 可以输出一个或多个字符串
* print 只允许输出一个字符串，返回值总为1
* echo 的输出速度比 print 快，echo 没有返回值，print 有返回值1

## 3.1  echo 语句

echo是一个语言结构，使用的时候可以不加括号，也可以加上括号。

``` php
<?php
    
echo "<h2>PHP很有趣<h2>";  //echo输出的字符串中可包含html标签
echo "Hello World!<br>";
echo "这是一个", "字符串，", "使用了", "多个参数。";  //可以一次输出多个字符串


$txt = "PHP";
$cars = array("Volvo", "BMW", "Toyota");

echo $txt;
echo "<br>";
echo "小彭正在学习$txt";
echo "她开着邻居家的{$cars[2]}追着日落。"

?>
```

## 3.2  print 语句

print 同样是一个语言结构，可以使用括号，也可以不使用括号。

``` php
<?php
$txt = "PHP";
$cars = array("Volvo", "BMW", "Toyota");

print $txt;
print "<br>";
print "小彭正在学习$txt";
print "她开着邻居家的{$cars[2]}追着日落。";

?>
```



# 4、EOF (heredoc) 使用说明

PHP  EOF 是一种在命令行shell 和 程序语言里定义一个字符串的方法。

使用概述如下：

* 必须后接分号，否则编译不通过。
* ``EOF``可以使用任意其他字符代替，只需保证**结束标识**和**开始标识**一致。
* **结束标识必须顶格肚子占一行，前后不能衔接任何空白和字符。**
* 开始标识可以不带引号或带单/双引号，不带引号和带双引号效果一致；解释内嵌的变量和转义符号；带单引号则不解释内嵌的变量和转义符号。
* 当内容需要内嵌引号（单/双引号）时，不需要加转义符，本身对单双引号转义。

``` php
<?php
$name = "pp";
$str = <<<EFO 
			$name"正在学PHP"
			"当代大学生太难了~"
EOF; 
echo $str;
?>
```

注意：

* 上述代码中``<<<EOF`` 是开始标识，``EOF``是结束标识。结束标识必须顶格写，且不能有缩进和空格，结束标识末尾要有分号。
* 保证开始标识和结束标识相同。
* 位于开始标识和结束标识之间的**变量**可以被正常解析，但**函数**则不行。、
* 变量不需要用``.``或者``,``来拼接。

# 5、PHP中的数据类型

PHP有以下数据类型：

* 字符串，String
* 整型，Integer
* 浮点型，Float
* 布尔型，Boolean
* 数组，Array
* 对象，Object
* 空值，NULL

## 5.1  String

在PHP的字符串类型中，我们可以将文本放在单引号或双引号中。

``` php
<?php
$str1 = "Hello World!";
$str2 = 'Hello World!';
?>
```

## 5.2  Integer

整数规则：

* 至少含有一个数字。
* 不含空格、逗号、小数点。
* 含正整数和负整数。
* 可以用一下三种格式指定：十进制、十六进制（以0x为前缀）、八进制（以0为前缀）

``` php
<?php
$x1 = 5985;  //十进制数
$x2 = 0x1761;  //十六进制数，其十进制的值为5985
$x3 = 013541;  //八进制数，其十进制的值为5985
?>
```

## 5.3  Float

浮点数是带小数部分的数字，或是指数形式。

``` php
<?php
$x = 10.365;
$y = 8e-5;
$z = 2.4e6;
?>
```

## 5.4  Boolean

布尔型的值可以是true或false.

``` php
<?php
$flag = true;
?>
```

## 5.5  数组

数组可以在一个变量中存储多个值。

```php
<?php
$cars = array("Volvo", "BWM", "Toyota");
?>
```

## 5.6  对象

必须用``class``关键字声明类，类可以包含**属性**和**方法**。

``` php
<?php
class Car {
    var $color;
    function _construct($color = "green") {
		$this->color = $color;  // this是指向当前对象实例的指针
    }
    function what_color() {
        return $this->color;
    }
}
?>
```

## 5.7  NULL

NULL表示空，即没有值。

``` php
<?php
$x = "Hello World!";
$x = null;
?>
```

# 6、类型比较



尽管PHP是弱类型语言，但我们仍经常需要对PHP变量进行比较。

* 松散比较：使用``==``进行比较，只比较值，不比较类型。
* 严格比较：使用``===``进行比较，既比较值，又比较类型。

``` php
<?php
if(42 == "42") {
    echo '1、值相等';
}
 
echo PHP_EOL; // 换行符
 
if(42 === "42") {
    echo '2、类型相等';
} else {
    echo '3、类型不相等';
}
/*
* 输出：
* 1、值相等
* 3、类型相等
*/
?>
```

**PHP中0、false、null的比较**

``` php
0 == false: true
0 === false: false

0 == null: true
0 === null: false

false == null: true
false === null: false

"0" == false: true
"0" === false: false

"0" == null: false
"0" === null: false

"" == false: true
"" === false: false

"" == null: true
"" === null: false
```



# 7、常量

常量是一个简单的标识符，它的值被定义后，在脚本中的任何地方都不能改变。

常量名由英文字母或、下划线、数字组成（数字不能做首字母），且常量名不需要加``$``修饰符。

**注意：常量在整个脚本中都可以使用。**

## 7.1  设置PHP常量

PHP中使用``define()``函数来设置常量，其函数语法如下：

``` php
bool define(string $name, mixed $value [, bool $case_insensitive = false])
```

* name ：必选参数，常量名称，即标识符。
* value ：必选参数，常量的值。
* case_insensitive ：可选参数，如果设置为true，则该常量大小写不敏感。默认大小写敏感。

``` php
<?php
define("GREETING", "Welcome!");
echo GREETING;  //输出：Welcome!
echo '<br>';
echo greeting;  //输出：greeting   ->  define函数默认大小写敏感
?>
```

## 7.2  常量是全局的

常量在定义后，默认是全局的，可以在整个脚本中任何地方使用（在函数中访问也不需要使用``global``关键字）。

``` php
<?php
define("MAX", 25, true);  //定义大小写不敏感的常量

function test() {
	echo max;  //可以不使用 global 关键字 
}

test();
?>
```

# 8、PHP字符串变量

为字符串变量赋值时，需要把文本加上单引号或双引号。

## 8.1  并置运算符

PHP中，只有一个字符串的运算符——并置运算符``.``

并置运算符用于把两个字符串连接起来。

``` php
<?php
$txt1 = "Hello, ";
$txt2 = "Peng.";
$txt3 = $txt1." ".$text2;  // 并置运算符可以连续使用多次

// 输出： Hello,Peng.
?>
```

## 8.2  strlen()函数

strlen()函数可以返回字符串的长度。

``` php
<?php
$len = strlen("Hello World!");
?>
```

## 8.3  strpos()函数

strpos()函数用于在字符串内查找一个字符或一段文本。

如果找到匹配的子串，该函数会返回第一个匹配的字符位置。否则返回false.

``` php
<?php
echo strpos("Hello World!", "World");
?>
```

## 8.4  其他函数

略

# 9、PHP运算符

## 9.1  算术运算符

| 运算符 | 名称 | 描述           |
| ------ | ---- | -------------- |
| x + y  | 加   | 求和           |
| x - y  | 减   | 做差           |
| x * y  | 乘   | 乘积           |
| x / y  | 除   | 求商           |
| x % y  | 取模 | 求余数         |
| - x    | 取反 | 取反           |
| a . b  | 并置 | 连接两个字符串 |

## 9.2  赋值运算符

| 运算符 | 等同于    | 描述                           |      |
| ------ | --------- | ------------------------------ | ---- |
| x = y  | x = y     | 将右侧表达式的值赋给左侧操作数 |      |
| x += y | x = x + y | 加                             |      |
| x -= y | x = x - y | 减                             |      |
| x *= y | x = x * y | 乘                             |      |
| x /= y | x = x / y | 除                             |      |
| x %= y | x = x % y | 取模                           |      |
| a .= b | a = a.b   | 连接两个字符串                 |      |

## 9.3  递增/递减运算符

| 运算符 | 名称   | 描述            |
| ------ | ------ | --------------- |
| ++ x   | 预递增 | x加1，然后返回x |
| x ++   | 后递增 | 返回x，然后x加1 |
| -- x   | 预递减 | x减1，然后返回x |
| x --   | 后递减 | 返回x，然后x减1 |

## 9.4  比较运算符

| 运算符  | 名称       | 描述                                       |
| ------- | ---------- | ------------------------------------------ |
| x == y  | 等于       | 如果x等于y，则返回true                     |
| x === y | 绝对等于   | 如果x等于y，且它们类型相同，则返回true     |
| x != y  | 不等于     | 如果x不等于y，则返回true                   |
| x <> y  | 不等于     | 如果x不等于y，则返回true                   |
| x !== y | 绝对不等于 | 如果x不等于y，或它们的类型不同，则返回true |
| x > y   | 大于       | 如果x大于y，则返回true                     |
| x < y   | 小于       | 如果x小于y，则返回true                     |
| x >= y  | 大于等于   | 如果x大于等于y，则返回true                 |
| x <= y  | 小于等于   | 如果x小于等于y，则返回true                 |

## 9.5  逻辑运算符

| 运算符   | 名称     | 描述                                   |
| -------- | -------- | -------------------------------------- |
| x and y  | 逻辑与   | 只有x和y都为true，才返回true           |
| x or y   | 逻辑或   | 如果x和y中至少一个为true，则返回true   |
| x xor y  | 逻辑异或 | 当x和y中有且只有一个为true，则返回true |
| x && y   | 与       | 只有x和y都为true，才返回true           |
| x \|\| y | 或       | 如果x和y中至少一个为true，则返回true   |
| !x       | 非       | 只有x不为true时返回true                |

## 9.6  数组运算符

| 运算符  | 名称   | 描述                                                     |
| ------- | ------ | -------------------------------------------------------- |
| x + y   | 集合   | 返回x和y的集合                                           |
| x == y  | 相等   | 如果x和y具有相同的键值对，则返回true                     |
| x === y | 恒等   | 如果x和y具有相同的键值对，且顺序相同类型相同，则返回true |
| x != y  | 不相等 | 如果x不等于y，则返回true                                 |
| x <> y  | 不相等 | 如果x不等于y，则返回true                                 |
| x !== y | 不恒等 | 如果x不等于y，则返回true                                 |

## 9.7  三元运算符

三元运算符`` ? :``是一种条件运算符。语法格式如下：

``` php
(exp1) ? (exp2) : (exp3)
```

实例：

```php
<?php
$test = 'Peng';
//普通写法
$username = isset($test) ? $test : 'nobody';

//PHP 5.3+ 写法：省略三元运算符的中间部分（exp2)
$username = $test ?: 'nobody';
?>
```

PHP 7+版本多了一个NULL合并运算符``??``，实例如下：

``` php
<?php
//如果$_GET['user']不存在，返回'nobody'，否则返回$_GET['user']的值
$username = $_GET['user'] ?? 'nobody';

//同效果的三元运算符
$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';
?>
```

## 9.8  组合比较符

PHP 7+支持组合比较符，也称之为太空船操作符，符号为``<=>``。组合比较运算符可以较为轻松地实现两个变量的比较，且不仅限于数值类数据的比较。语法格式如下：

``` php
$c = $a <=> $b;
```

解析：

* 如果a > b，则c的值为1.
* 如果a == b，则c的值为0.
* 如果a < b，则c的值为-1.

``` php
<?php
// 整型
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1
 
// 浮点型
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
?>
```

# 10、条件语句

在PHP中，提供了下列条件语句：

* **if 语句**
* **if...else 语句**
* **if...else if...else 语句**
* **switch 语句**

## 10.1  if 语句

语法格式如下：

```php
if(条件) {
	条件成立时要执行的代码
}
```

## 10.2  if...else 语句

语法格式如下：

``` php
if(条件) {
	条件成立时执行的代码
} else {
    条件不成立时执行的代码
}

```

## 10.3  if...else if...else 语句

若干条件之一成立时执行一个代码块。语法格式如下：

``` php
if(条件1) {
	条件1成立时执行的代码
} else if(条件2) {
    条件2成立时执行的代码
} else {
    以上条件都不成立时执行的代码
}
```

## 10.4  switch语句

switch语句用于根据多个不同条件执行不同的动作。

``` php
<?php
switch (n)
{
case label1:
    如果 n=label1，此处代码将执行;
    break;
case label2:
    如果 n=label2，此处代码将执行;
    break;
default:
    如果 n 既不等于 label1 也不等于 label2，此处代码将执行;
}
?>
```

工作原理：首先对一个简单的表达式 *n*（通常是变量）进行一次计算。将表达式的值与结构中每个 case 的值进行比较。如果存在匹配，则执行与 case 关联的代码。代码执行后，使用 **break** 来阻止代码跳入下一个 case 中继续执行。**default** 语句用于不存在匹配（即没有 case 为真）时执行。

# 11、PHP数组

数组是一种能在单个变量中存储多个值的特殊变量。

## 11.1  数组的创建

PHP使用函数``array()``来创建数组。

在PHP中有以下三种数组：

* **数值数组**：带有数字ID键的数组。
* **关联数组**：带有指定键的数组，每个键关联一个值。
* **多维数组**：包含一个或多个数组。

## 11.2  数值数组

创建数值数组有两种方式：自动分配ID键、人工分配ID键。

``` php
<?php
// 自动分配ID键	
$cars = array("Volvo", "BMW", "Toyota");

//人工分配ID键
$books[0] = "《剑来》";
$books[1] = "《龙族》";
?>
```

可以通过``count()``函数来获取数组长度。

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
?>
```

使用for循环遍历数组：

```php
<?php
$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);
 
for($x=0;$x<$arrlength;$x++)
{
    echo $cars[$x];
    echo "<br>";
}
?>
```

## 11.3  关联数组

创建关联数组的方式也有两种：

```php
<?php
//方式1
$age1 = array("Peter"=>"35", "Ben"=>"37", "Joe"=>"43");

//方式2
$age2['Peter'] = 35;
$age2['Ben'] = 37;
$age2['Joe'] = 43;

echo "Peter is ".$age['Peter']." years old.";
?>
```

使用foreach循环遍历数组：

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```

## 11.4  多维数组

一个数组中的值可以是另一个数组，另一个数组的值也可以是一个数组。

```php
<?php
//二维数组的创建(数值数组)
$cars = arrar
(
    array("Volvo", 100, 96),
    array("BMW", 60, 59),
    array("Toyota",110,100)
);

//二维数组的创建（关联数组）
$sites = array
(
    "runoob"=>array
    (
        "菜鸟教程",
        "http://www.runoob.com"
    ),
    "google"=>array
    (
        "Google 搜索",
        "http://www.google.com"
    ),
    "taobao"=>array
    (
        "淘宝",
        "http://www.taobao.com"
    )
);

?>
```



## 11.5  数组的排序

PHP中有以下针对数组的排序函数：

* ``sort()`` -> 对数组进行升序排序
* ``rsort()`` -> 对数组进行降序排序
* ``asort()`` -> 根据关联数组的值，对数组进行升序排序
* ``ksort()`` -> 根据关联数组的键，对数组进行升序排序
* ``arsort()`` -> 根据关联数组的值，对数组进行降序排序
* ``krsort()`` -> 根据关联数组的键，对数组进行降序排序

# 12、超级全局变量

PHP中预定义了几个超级全局变量（superglobals），它们在一个脚本的全部作用域中都可用，且你不需要特别说明，就可以在函数及类中使用。

PHP超级全局变量列表：

* **$GLOBALS**
* **$_SERVER**
* **$_REQUEST**
* **$_POST**
* **$_GET**
* **$_FILES**
* **$_ENV**
* **$_COOKIE**
* **$_SESSION**

## 12.1  $GLOBALS

$GLOBALS是一个包含了全部变量的全局组合数组，变量的名字就是数组的键。

``` php
<?php
$x = 75;
$y = 25;

function addtion() {
    $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
}
addtion();
echo $z;

// 输出；100
?>
```

## 12.2  $_SERVER

$_SERVER是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等信息的数组。

下表是$_SERVER中的重要元素。

| 元素/代码                       | 描述                                                         |
| :------------------------------ | :----------------------------------------------------------- |
| $_SERVER['PHP_SELF']            | 当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。__FILE__ 常量包含当前(例如包含)文件的完整路径和文件名。 从 PHP 4.3.0 版本开始，如果 PHP 以命令行模式运行，这个变量将包含脚本名。之前的版本该变量不可用。 |
| $_SERVER['GATEWAY_INTERFACE']   | 服务器使用的 CGI 规范的版本；例如，"CGI/1.1"。               |
| $_SERVER['SERVER_ADDR']         | 当前运行脚本所在的服务器的 IP 地址。                         |
| $_SERVER['SERVER_NAME']         | 当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.runoob.com) |
| $_SERVER['SERVER_SOFTWARE']     | 服务器标识字符串，在响应请求时的头信息中给出。 (如：Apache/2.2.24) |
| $_SERVER['SERVER_PROTOCOL']     | 请求页面时通信协议的名称和版本。例如，"HTTP/1.0"。           |
| $_SERVER['REQUEST_METHOD']      | 访问页面使用的请求方法；例如，"GET", "HEAD"，"POST"，"PUT"。 |
| $_SERVER['REQUEST_TIME']        | 请求开始时的时间戳。从 PHP 5.1.0 起可用。 (如：1377687496)   |
| $_SERVER['QUERY_STRING']        | query string（查询字符串），如果有的话，通过它进行页面访问。 |
| $_SERVER['HTTP_ACCEPT']         | 当前请求头中 Accept: 项的内容，如果存在的话。                |
| $_SERVER['HTTP_ACCEPT_CHARSET'] | 当前请求头中 Accept-Charset: 项的内容，如果存在的话。例如："iso-8859-1,*,utf-8"。 |
| $_SERVER['HTTP_HOST']           | 当前请求头中 Host: 项的内容，如果存在的话。                  |
| $_SERVER['HTTP_REFERER']        | 引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。) |
| $_SERVER['HTTPS']               | 如果脚本是通过 HTTPS 协议被访问，则被设为一个非空的值。      |
| $_SERVER['REMOTE_ADDR']         | 浏览当前页面的用户的 IP 地址。                               |
| $_SERVER['REMOTE_HOST']         | 浏览当前页面的用户的主机名。DNS 反向解析不依赖于用户的 REMOTE_ADDR。 |
| $_SERVER['REMOTE_PORT']         | 用户机器上连接到 Web 服务器所使用的端口号。                  |
| $_SERVER['SCRIPT_FILENAME']     | 当前执行脚本的绝对路径。                                     |
| $_SERVER['SERVER_ADMIN']        | 该值指明了 Apache 服务器配置文件中的 SERVER_ADMIN 参数。如果脚本运行在一个虚拟主机上，则该值是那个虚拟主机的值。(如：someone@runoob.com) |
| $_SERVER['SERVER_PORT']         | Web 服务器使用的端口。默认值为 "80"。如果使用 SSL 安全连接，则这个值为用户设置的 HTTP 端口。 |
| $_SERVER['SERVER_SIGNATURE']    | 包含了服务器版本和虚拟主机名的字符串。                       |
| $_SERVER['PATH_TRANSLATED']     | 当前脚本所在文件系统（非文档根目录）的基本路径。这是在服务器进行虚拟到真实路径的映像后的结果。 |
| $_SERVER['SCRIPT_NAME']         | 包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。 |
| $_SERVER['SCRIPT_URI']          | URI 用来指定要访问的页面。例如 "/index.html"。               |



$_SERVER中元素的使用：

```php
<?php
echo $_SERVER['PHP_SELF'];
echo $_SERVER['HTTP_HOST'];
?>
```

## 12.3  $_REQUEST

$_REQUEST用于收集HTML表单提交的数据。

```php+HTML
<html>
<body>
 
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">  <!--将表单提交给当前PHP脚本进行处理-->
Name: <input type="text" name="fname">
<input type="submit">
</form>
 
<?php 
$name = $_REQUEST['fname'];  //获取所提交表单的信息
echo $name; 
?>
 
</body>
</html>
```

## 12.4  $_POST

$_POST被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="post"。

```php+HTML
<html>
<body>
 
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>
 
<?php 
$name = $_POST['fname']; 
echo $name; 
?>
 
</body>
</html>
```

## 12.5  $_GET

PHP $_GET 同样被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="get"。

$_GET 也可以收集URL中发送的数据。

```php+HTML
<html>
<body>

<a href="test_get.php?subject=PHP&web=runoob.com">Test $GET</a>
    
</body>
</html>
```

以下是test_get.php文件：

```php
<html>
<body>
 
<?php 
echo "Study " . $_GET['subject'] . " @ " . $_GET['web'];
?>
 
</body>
</html>
```

# 13、循环语句

PHP提供了以下循环语句：

* **while**
* **do...while**
* **for**
* **foreach**

## 13.1  while循环

```php
<?php

$i = 1;
while($i < 10) {
    echo "the number is ".$i." .";
    echo "<br>";
    $i++;
}

?>
```

## 13.2  do...while语句

```php
<?php
$i=1;
do
{
    $i++;
    echo "The number is " . $i . "<br>";
}
while ($i<=5);
?>

```

## 13.3  for 循环

```php
<?php
for ($i=1; $i<=5; $i++)
{
    echo "数字为 " . $i . PHP_EOL;
}
?>
```

## 13.4  foreach 循环

foreach 循环用于遍历数组。

语法如下：

遍历数值数组：

```php
foreach ($array as $value)
{
    要执行代码;
}
```

遍历关联数组：

```php
foreach ($array as $key => $value)
{
    要执行代码;
}
```

实例如下：

```php
<?php
//遍历数值数组
$x=array("Google","Runoob","Taobao");
foreach ($x as $value)
{
    echo $value . PHP_EOL;
}

//遍历关联数组
$y=array(1=>"Google", 2=>"Runoob", 3=>"Taobao");
foreach ($y as $key => $value)
{
    echo "key  为 " . $key . "，对应的 value 为 ". $value . PHP_EOL;
}

?>
```

# 14、函数

PHP的真正威力源于它的函数——PHP提供了超过1000个内建函数。

关于内建函数，参考：[PHP内建函数](https://www.runoob.com/php/php-ref-array.html)

### 自定义函数

```php
<?php
function add($x,$y)  //function关键字、函数名、形参
{
    $total=$x+$y;
    return $total;  //如果函数有返回值则使用return语句，没有则省略
}
 
echo "1 + 16 = " . add(1,16);
?>
```



# 15、魔术常量

PHP提供了大量的预定义常量，不过很多常量都是由不同的扩展库提供的，只有在加载了这些扩展库时才会出现。

有8个魔术常量，他们的值随着它们在代码中的位置的改变而改变。

## 15.1  _ LINE_

文件中当前行号。

```php
<?php
echo '这是第 " '  . __LINE__ . ' " 行';
?>
```

## 15.2  _ FILE_

文件完成的路径和文件名。若果用在被包含文件中，则返回被包含文件名。

```php
<?php
echo '该文件位于 " '  . __FILE__ . ' " ';
?>
```

## 15.3  _ DIR_

文件所在的目录。

```php
<?php
echo '该文件位于 " '  . __DIR__ . ' " ';
?>
```

## 15.4  _ FUNCTION_

函数名称。返回该函数被定义时的名字。

```php
<?php
function test() {
    echo  '函数名为：' . __FUNCTION__ ;
}
test();
?>
```

## 15.5  _ CLASS_

类的名称。返回类被定义时的名称。

```php
<?php
class test {
    function _print() {
        echo '类名为：'  . __CLASS__ . "<br>";
        echo  '函数名为：' . __FUNCTION__ ;
    }
}
$t = new test();
$t->_print();
?>
```

## 15.6  _ TRAIT_

php从以前到现在一直都是单继承的语言，无法同时从两个基类中继承属性和方法，为了解决这个问题，PHP新增了Trait这个特性。

用法：通过在类中使用use 关键字，声明要组合的Trait名称，具体的Trait的声明使用Trait关键词，Trait不能实例化.

一个类可以组合多个Trait，通过逗号相隔。

Trait中的方法会覆盖 基类中的同名方法，而本类会覆盖Trait中同名方法。

 注意：当trait定义了属性后，类就不能定义同样名称的属性，否则会产生 fatal error，除非是设置成相同可见度、相同默认值。

```php
<?php
trait Dog{
    public $name="dog";
    public function drive(){
        echo "This is dog drive";
    }
    public function eat(){
        echo "This is dog eat";
    }
}

class Animal{
    public function drive(){
        echo "This is animal drive";
    }
    public function eat(){
        echo "This is animal eat";
    }
}

class Cat extends Animal{
    use Dog;
    public function drive(){
        echo "This is cat drive";
    }
}
$cat = new Cat();
$cat->drive();  //输出：This is cat drive
echo "<br/>";
$cat->eat();  //输出：This is dog eat.

?>
```

## 15.7  _ METHOD_

类的方法名。返回该方法被定义时的名字（区分大小写）。

```php
<?php
function test() {
    echo  '函数名为：' . __METHOD__ ;
}
test();
?>
```

## 15.8  _ NAMESPACE_

当前命名空间的名称。

```php
<?php
namespace MyProject;
 
echo '命名空间为："', __NAMESPACE__, '"'; // 输出 "MyProject"
?>
```

# 16、命名空间（namespace）

命名空间用于解决以下两类问题：

1. 用户代码与PHP内部的类/函数/常量或第三方类/函数/常量之间的名字冲突。
2. 为很长的标识符名称创建一个别名，提高源码的可读性。

## 16.1  定义命名空间

* 默认情况下，所有常量、类和函数都放在全局空间下，就和PHP支持命名空间之前一样。
* 命名空间通过关键字``namespace``来声明。如果一个文件中包含命名空间，它必须在其他所有代码之前声明命名空间。
* 可以在同一个文件中定义不同的命名空间，建议用大括号包裹命名空间。
* 将全局的非命名空间中的代码与命名空间中的代码组合在一起，只能使用大括号形式的语法。全局代码必须用一个不带名称的 namespace 语句加上大括号括起来。
* 在声明命名空间之前唯一合法的代码是用于定义源文件编码方式的 declare 语句。所有非 PHP 代码包括空白符都不能出现在命名空间的声明之前。

```php
<?php
declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
namespace MyProject {
    const CONNECT_OK = 1;
    class Connection { /* ... */ }
    function connect() { /* ... */  }
}

namespace AnotherProject {  //在同一文件中声明多个命名空间
    const CONNECT_OK = 1;
    class Connection { /* ... */ }
    function connect() { /* ... */  }
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}
?>
```

## 16.2  子命名空间

与目录和文件的关系类似，，PHP命名空间也允许指定层次化的命名空间名称。

```php
<?php
namespace MyProject\Sub\Level;  //声明分层次的单个命名空间

const CONNECT_OK = 1;
class Connection { /* ... */ }
function Connect() { /* ... */  }

?>
```

## 16.3  命名空间的使用

1. 非限定名称，或不包含前缀的名称。
2. 限定名称，或包含前缀的名称。
3. 完全限定名称，或包含了全局前缀操作符的名称。

实例如下：

file1.php文件代码

 ```php
<?php
namespace Foo\Bar\subnamespace; 

const FOO = 1;
function foo() {}
class foo
{
    static function staticmethod() {}
}
?>
 ```

file2.php文件代码

```php
<?php
namespace Foo\Bar;
include 'file1.php';

const FOO = 2;
function foo() {}
class foo
{
    static function staticmethod() {}
}

/* 非限定名称 */
foo(); // 解析为函数 Foo\Bar\foo
foo::staticmethod(); // 解析为类 Foo\Bar\foo ，方法为 staticmethod
echo FOO; // 解析为常量 Foo\Bar\FOO

/* 限定名称 */
subnamespace\foo(); // 解析为函数 Foo\Bar\subnamespace\foo
subnamespace\foo::staticmethod(); // 解析为类 Foo\Bar\subnamespace\foo,
                                  // 以及类的方法 staticmethod
echo subnamespace\FOO; // 解析为常量 Foo\Bar\subnamespace\FOO
                                  
/* 完全限定名称 */
\Foo\Bar\foo(); // 解析为函数 Foo\Bar\foo
\Foo\Bar\foo::staticmethod(); // 解析为类 Foo\Bar\foo, 以及类的方法 staticmethod
echo \Foo\Bar\FOO; // 解析为常量 Foo\Bar\FOO
?>
```

# 17、面向对象

## 17.1  面向对象的内容

- **类** − 定义了一件事物的抽象特点。类的定义包含了数据的形式以及对数据的操作。
- **对象** − 是类的实例。
- **成员变量** − 定义在类内部的变量。该变量的值对外是不可见的，但是可以通过成员函数访问，在类被实例化为对象后，该变量即可称为对象的属性。
- **成员函数** − 定义在类的内部，可用于访问对象的数据。
- **继承** − 继承性是子类自动共享父类数据结构和方法的机制，这是类之间的一种关系。在定义和实现一个类的时候，可以在一个已经存在的类的基础之上来进行，把这个已经存在的类所定义的内容作为自己的内容，并加入若干新的内容。
- **父类** − 一个类被其他类继承，可将该类称为父类，或基类，或超类。
- **子类** − 一个类继承其他类称为子类，也可称为派生类。
- **多态** − 多态性是指相同的函数或方法可作用于多种类型的对象上并获得不同的结果。不同的对象，收到同一消息可以产生不同的结果，这种现象称为多态性。
- **重载** − 简单说，就是函数或者方法有同样的名称，但是参数列表不相同的情形，这样的同名不同参数的函数或者方法之间，互相称之为重载函数或者方法。
- **抽象性** − 抽象性是指将具有一致的数据结构（属性）和行为（操作）的对象抽象成类。一个类就是这样一种抽象，它反映了与应用有关的重要性质，而忽略其他一些无关内容。任何类的划分都是主观的，但必须与具体的应用有关。
- **封装** − 封装是指将现实世界中存在的某个客体的属性与行为绑定在一起，并放置在一个逻辑单元内。
- **构造函数** − 主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中。
- **析构函数** − 析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。析构函数往往用来做"清理善后" 的工作（例如在建立对象时用new开辟了一片内存空间，应在退出前在析构函数中用delete释放）。

## 17.2  类定义

类定义的方式如下：

- 类使用 **class** 关键字后加上类名定义。
- 类名后的一对大括号({})内可以定义变量和方法。
- 类的变量使用 **var** 来声明, 变量也可以初始化值。
- 函数定义类似 PHP 函数的定义，但函数（静态函数除外）只能通过该类及其实例化的对象访问。

实例如下：

```php
<?php
class Site {
  /* 成员变量 */
  var $url;
  var $title = "Peng";
  
  /* 成员函数 */
  function setUrl($par){
     $this->url = $par;  // $this 代表该对象自身
  }
  
  function getUrl(){
     echo $this->url . PHP_EOL;
  }
  
  function setTitle($par){
     $this->title = $par;
  }
  
  function getTitle(){
     echo $this->title . PHP_EOL;
  }
}
?>
```

## 17.3  对象的创建

PHP使用``new``运算符来实例化一个类的对象。

```php
$runoob = new Site;
```

## 17.4  成员属性和方法的调用

实例化对象后，我们可以通过``->``操作符调用对象的属性和方法。

```php
// 调用成员函数
$runoob->setTitle( "菜鸟教程" );

//调用成员属性
$str = $runoob->url;
```

**注意：**

在访问PHP类中的成员变量或方法时，如果被引用的变量或者方法被声明成const或者static,那么就必须使用操作符``::``
反之如果被引用的变量或者方法没有被声明成const或者static,那么就必须使用操作符``->``。

另外，如果从类的内部访问const或者static变量或者方法,那么就必须使用自引用的``self``，
反之如果从类的内部访问不为const或者static变量或者方法,那么就必须使用自引用的``$this``

## 17.5  构造函数

构造函数是一种特殊的方法。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，在创建对象的语句中与 **new** 运算符一起使用。

定义构造函数的语法格式如下：

```php
void _construct([ mixed $args [, $...]])
```

在上述实例中调用构造函数来初始化$url和$title变量：

```php
function _construct($par1, $par2) {
	$this->url = $par1;
    $this->title = $par2;
}
```

## 17.6  析构函数

析构函数(destructor) 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。

语法格式如下：

```php
void _destruct(void)
```

实例如下：

```php
<?php
class MyDestructableClass {
	function _construct() {
        print "构造函数\n";
    }
    
    function _destruct() {
		print "析构函数\b";
    }
}
$obj = new MyDestructableClass();

?>
```

## 17.7  继承

在PHP中，我们使用``extends``关键字来继承一个类。PHP不支持多继承。

格式如下：

```php
class Child extends Parent {
    ...
}
```

实例如下：

```php
<?php 
// Child_Site类继承了Site类，并扩展了功能
class Child_Site extends Site {
   var $category;

    function setCate($par){
        $this->category = $par;
    }
  
    function getCate(){
        echo $this->category . PHP_EOL;
    }
}
```

## 17.8  方法的重写

如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。

```php
<?php 
// Child_Site类继承了Site类，并扩展了功能、重写了部分方法
class Child_Site extends Site {
   var $category;

    function setCate($par){
        $this->category = $par;
    }
  
    function getCate(){
        echo $this->category . PHP_EOL;
    }
    
    function getUrl() {
       echo $this->url . PHP_EOL;
       return $this->url;
	}
   
    function getTitle(){
       echo $this->title . PHP_EOL;
       return $this->title;
    }
    
}
```

## 17.9  访问控制

PHP 对属性或方法的访问控制，是通过在前面添加关键字： public（公有），protected（受保护）或 private（私有）来实现的。

- **public（公有）：**公有的类成员可以在任何地方被访问。
- **protected（受保护）：**受保护的类成员则可以被其自身以及其子类和父类访问。
- **private（私有）：**私有的类成员则只能被其定义所在的类访问。

类属性必须定义为public，protected，private之一。如果用 var 定义，则被视为public.

类方法可以定义为public，protected，private之一。如果没有设置这些关键字，则默认为public.

```php
<?php
/**
 * Define MyClass
 */
class MyClass
{
    public $public = 'Public';
    protected $protected = 'Protected';
    private $private = 'Private';

    // 声明一个公有的构造函数
    public function __construct() { }

    // 声明一个公有的方法
    public function MyPublic() { }

    // 声明一个受保护的方法
    protected function MyProtected() { }

    // 声明一个私有的方法
    private function MyPrivate() { }

    // 此方法为公有
    function Foo()
    {
        $this->MyPublic();
        $this->MyProtected();
        $this->MyPrivate();
    }
}

$obj = new MyClass();
echo $obj->public; // 这行能被正常执行
echo $obj->protected; // 这行会产生一个致命错误
echo $obj->private; // 这行也会产生一个致命错误
$obj->printHello(); // 输出 Public、Protected 和 Private

$obj->MyPublic(); // 这行能被正常执行
$obj->MyProtected(); // 这行会产生一个致命错误
$obj->MyPrivate(); // 这行会产生一个致命错误
$obj->Foo(); // 公有，受保护，私有都可以执行


/**
 * Define MyClass2
 */
class MyClass2 extends MyClass
{
    // 可以对 public 和 protected 进行重定义，但 private 而不能
    protected $protected = 'Protected2';

    function printHello()
    {
        echo $this->public;
        echo $this->protected;
        echo $this->private;
    }
}

$obj2 = new MyClass2();
echo $obj2->public; // 这行能被正常执行
echo $obj2->private; // 未定义 private
echo $obj2->protected; // 这行会产生一个致命错误
$obj2->printHello(); // 输出 Public、Protected2 和 Undefined

$obj2 = new MyClass2;
$obj2->MyPublic(); // 这行能被正常执行
$obj2->Foo2(); // 公有的和受保护的都可执行，但私有的不行

?>
```

## 17.10  接口

使用接口（interface），可以指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容。

接口是通过 **interface** 关键字来定义的，就像定义一个标准的类一样，但其中定义所有的方法都是空的。

接口中定义的所有方法都必须是公有，这是接口的特性。

要实现一个接口，使用 **implements** 操作符。类中必须实现接口中定义的所有方法，否则会报一个致命错误。类可以实现多个接口，用逗号来分隔多个接口的名称。

## 17.11  常量

可以把在类中始终保持不变的值定义为常量。使用``const``关键字来**在类中**定义常量（define()用于定义全局常量，不能用于类中），且在定义和使用常量的时候不需要使用 $ 符号。

常量的值必须是一个定值，不能是变量，类属性，数学运算的结果或函数调用。

自 PHP 5.3.0 起，可以用一个变量来动态调用类。但该变量的值不能为关键字（如 self，parent 或 static）。

```php
<?php
class MyClass
{
    const constant = '常量值';

    function showConstant() {
        echo  self::constant . PHP_EOL;
    }
}

echo MyClass::constant . PHP_EOL;

$classname = "MyClass";
echo $classname::constant . PHP_EOL; // 自 5.3.0 起

$class = new MyClass();
$class->showConstant();

echo $class::constant . PHP_EOL; // 自 PHP 5.3.0 起
?>
```

## 17.12  抽象类

任何一个类，如果它里面至少有一个方法是被声明为抽象的，那么这个类就必须被声明为抽象的。

使用``abstract``关键字来定义抽象类和抽象方法。

定义为抽象的类不能被实例化。

被定义为抽象的方法只是声明了其调用方式（参数），不能定义其具体的功能实现。

继承一个抽象类的时候，子类必须定义父类中的所有抽象方法；另外，这些方法的访问控制必须和父类中一样（或者更为宽松）。例如某个抽象方法被声明为受保护的，那么子类中实现的方法就应该声明为受保护的或者公有的，而不能定义为私有的。

## 17.13  static关键字

声明类属性或方法为 static(静态)，就可以不实例化类而直接访问。

静态属性不能通过一个类已实例化的对象来访问（但静态方法可以）。

由于静态方法不需要通过对象即可调用，所以伪变量 $this 在静态方法中不可用。

静态属性不可以由对象通过 -> 操作符来访问。

自 PHP 5.3.0 起，可以用一个变量来动态调用类。但该变量的值不能为关键字 self，parent 或 static.

```php
<?php
class Foo {
  public static $my_static = 'foo';
  
  public function staticValue() {
     return self::$my_static;
  }
}

print Foo::$my_static . PHP_EOL;
$foo = new Foo();

print $foo->staticValue() . PHP_EOL;
?>
```

## 17.14  final关键字

如果父类中的方法被声明为 final，则子类无法覆盖该方法。如果一个类被声明为 final，则不能被继承。

**注意：final关键字只能用来修饰类和方法！**

## 17.15  调用父类构造方法

PHP 不会在子类的构造方法中自动的调用父类的构造方法。要执行父类的构造方法，需要在子类的构造方法中调用 **parent::__construct()** 。

```php
<?php
class BaseClass {
   function __construct() {
       print "BaseClass 类中构造方法" . PHP_EOL;
   }
}
class SubClass extends BaseClass {
   function __construct() {
       parent::__construct();  // 子类构造方法不能自动调用父类的构造方法
       print "SubClass 类中构造方法" . PHP_EOL;
   }
}
class OtherSubClass extends BaseClass {
    // 继承 BaseClass 的构造方法
}

// 调用 BaseClass 构造方法
$obj = new BaseClass();

// 调用 BaseClass、SubClass 构造方法
$obj = new SubClass();

// 调用 BaseClass 构造方法
$obj = new OtherSubClass();
?>
```

