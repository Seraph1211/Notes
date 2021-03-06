## 1、AES简介

高级加密标准（Advanced Encryption Standard, 简称AES）是最为常见的一种**对称加密算法**，其加密过程涉及到4种操作：**字节替代**（SubBytes)、**行移位**（ShiftRows）、**列混淆**（MixColumns）和**轮密钥加**（AddRoundKey）。

其解密过程为别为对应的逆操作。由于每一步操作都是可逆的，按照相反的顺序进行解密即可恢复明文。

上面提到的**对称加密算法**是一种加密、解密使用相同密钥的加密算法，其特点是**算法公开、计算量小、加密速度快、加密效率高**。对称加密算法具体的加密流程如下图：

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcwMjE5MDgyOTA5Njg4?x-oss-process=image/format,png)



## 2、AES的加密过程

AES是分组加密的，也就是把明文分成若干组，每组长度相等，每次加密一组数据，直到加密完整个明文。其加密过程是在一个4×4的字节矩阵上运作的，矩阵初值是一个明文块（也就是明文的一个分组）。

加密时根据密钥的长度，加密的轮数也会有变化（每一个明文块都要经过n轮加密），比如当密钥长度为128位时，加密轮数为10轮。

每一轮（最后一轮除外）加密均包含4个步骤：字节替代、行移位、列混淆、轮密钥加。最后一轮加密循环省略列混淆这一步骤。

![image-20210508155947708](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508155947708.png)

### 2.1  字节替代（SubBytes）

字节替代实际上就是一个查表操作：通过查表的方式把状态矩阵中的每一个字节映射成新的字节。

![image-20210508152821137](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508152821137.png)节的高4位作为行值，低4位作为列值，然后取出S盒或逆S盒中对应的元素作为输出。

### 2.2  行移位（ShiftRows）

将矩阵中每一行的各个字节循环左移，位移量随着行数递增而递增。

比如当密钥长度为128比特时，状态矩阵第0行左移0字节，第1行左移1字节，第2行左移2字节，第3行左移3字节。如下图所示：

![image-20210508152753291](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508152753291.png)

### 2.3  列混淆（MixColumns）

列混淆是通过矩阵的乘法来实现的，经行移位后的状态矩阵与固定的矩阵相乘，得到混淆后的状态矩阵。

![image-20210508154413261](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508154413261.png)

### 2.4  轮密钥加（AddRoundKey）

轮密钥加是将矩阵中的每一个字节都与该轮次的密钥逐字做异或运算（XOR），每一轮次的密钥都由密钥生成方案产生。

![image-20210508154734544](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508154734544.png)



## 3、AES的工作模式

### 3.1  ECB（Electronic Code Book, 电子密码本）模式

ECB模式是最早采用、最简单的模式，它将加密的数据分成若干组，每组的大小跟密钥长度相同，然后每组明文都用相同的密钥进行加密。

![image-20210508161534029](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508161534029.png)

#### ECB模式的优点：

1. 简单。
2. 有利于并行计算。
3. 误差不会被传送。

#### ECB模式的缺点：

1. 不能隐藏明文的模式。
2. 可能对明文进行主动攻击。

ECB模式由于每块数据的加密是独立的因此加密和解密都可以并行计算，其最大的缺点是相同的明文块会被加密成相同的密文块，这种方法在某些环境下不能提供严格的数据保密性。

下面的例子显示了ECB在密文中显示平文的模式的程度：该图像的一个位图版本（左图）通过ECB模式可能会被加密成中图，而非ECB模式通常会将其加密成右图：

![image-20210508162623025](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508162623025.png)



### 3.2  CBC（Cipher-block chaining, 分组密码链接）模式

CBC模式对于每个待加密的密码块在加密前会先与前一个密码块的密文异或然后再用加密器加密。第一个明文块与一个叫初始化向量的数据块异或。

![image-20210508163307248](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508163307248.png)

#### CBC模式的优点：

1. 不容易主动攻击，安全性强于ECB模式。
2. 适合传输长度较长的报文。

**补充说明：**

人为攻击一般分为被动攻击和主动攻击。

被动攻击一种是指直接获取消息的内容，还有一种是对消息的某些特征进行分析，虽然不能得到完整的消息内容但也可以推测出信息的一些特点，而这些特点有可能是通信双方不想被泄露的。

主动攻击是指对明文数据的篡改来产生对攻击有价值的密文数据，防止主动攻击一般都非常困难，需要提前预防，要求在安全架构层面需要更加专业的知识和经验。

#### CBC模式的缺点：

1. 不利于并行计算。
2. 误差传送：一个明文损坏会影响多个单元。
3. 第一个明文块需要与一个初始化向量IV进行抑或，初始化向量IV的选取比较复杂。



### 3.3  CFB（Cipher feedback, 密文反馈）模式

CFB的加密工作分为两部分：

1. 将一前段加密得到的密文再加密.
2. 将第1步加密得到的数据与当前段的明文异或。

![CFB模式](D:\MyGitHouse\Notes\AES加密算法浅析.assets\CFB模式.jpg)

#### CFB模式的优点：

1. 隐藏了明文模式。
2. 分组密码转化为流模式。
3. 可以及时加密传输小于分组的数据。

#### CFB模式缺点：

1. 不利于并行计算。
2. 误差传送：一个明文损坏会影响多个单元。
3. 第一个明文块需要与一个初始化向量IV进行抑或，初始化向量IV的选取比较复杂。



### 3.4  OFB（Output feedback, 输出反馈）模式

OFB模式不是通过密码算法对明文直接加密的，而是通过将“明文分组”和“密码算法的输出”进行XOR运算来产生“密文分组”的。

![OFB模式](D:\MyGitHouse\Notes\AES加密算法浅析.assets\OFB模式.jpg)

#### CFB模式的优点：

1. 隐藏了明文模式。
2. 分组密码转化为流模式。
3. 可以及时加密传输小于分组的数据。

#### CFB模式缺点：

1. 不利于并行计算。
2. 误差传送：一个明文损坏会影响多个单元。
3. 对明文的主动攻击是可能的。



### 3.5  CTR（Counter, 计数器）模式

CTR模式中，每个分组对应一个逐次累加的计数器，并通过对计数器进行加密来生成密钥流。也就是说，最终的密文分组是通过将计数器加密而得到的比特序列，与明文分组进行XOR运算而得到的。

![image-20210508172728796](D:\MyGitHouse\Notes\AES加密算法浅析.assets\image-20210508172728796.png)

#### CTR模式优点：

1. 不需要填充。
2. 可事先进行加密、解密的准备。
3. 加密、解密使用相同的结构。
4. 对包含某些错误比特的密文进行解密时，只有明文中相对应的比特会出错。
5. 支持并行计算（加密、解密）。

#### CTR模式缺点：

主动攻击者反转密文分组中的某些比特时，明文分组中对应的比特也会被反转