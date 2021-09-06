#### 创建Git SSH密钥

1、启动Git Brash, 执行``cd ~/.ssh``命令进入.ssh目录，然后执行``ls``命令检查是否存在SSH密钥

![image-20210906162714681](D:\MyGitHouse\Notes\Sourcetree SSH密钥配置.assets\image-20210906162714681.png)

2、如果没有.ssh目录或者该目录下没有密钥文件，则执行``ssh-keygen -t rsa -C"your email"``命令创建SSH密钥

![image-20210906163231592](D:\MyGitHouse\Notes\Sourcetree SSH密钥配置.assets\image-20210906163231592-1630943302638.png)

#### Github配置SSH密钥

1、登录Github，进入Setting => SSH and GPG keys, 点击 New SSH key

![image-20210906164051036](D:\MyGitHouse\Notes\Sourcetree SSH密钥配置.assets\image-20210906164051036.png)

2、将.ssh目录下id_rsa.pub文件的内容复制到key中，并为密钥设置一个title

![image-20210906163839264](D:\MyGitHouse\Notes\Sourcetree SSH密钥配置.assets\image-20210906163839264.png) 

#### Sourcetree配置SSH密钥

打开Sourcetree客户端，进入工具 => 选项，配置用户信息和SSH客户端信息。

![image-20210906181747307](D:\MyGitHouse\Notes\Sourcetree SSH密钥配置.assets\image-20210906181747307.png)







