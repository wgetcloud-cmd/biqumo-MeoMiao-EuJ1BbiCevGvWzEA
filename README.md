
目录* [DH、ECDH 和 ECDHE 的关系](https://github.com)
* [Flow chart](https://github.com)
* [Reference](https://github.com):[veee加速器](https://youhaochi.com)



---


背景：
对称加解密算法都需要一把秘钥，但是很多情况下，互联网环境不适合传输这把对称密码，有被中间人拦截的风险。
为了解决这个问题，我们看看ECDH秘钥交换算法是怎么做的？


# DH、ECDH 和 ECDHE 的关系



> DH、ECDHE不是本文的重点， 知道即可。


Diffie\-Hellman密钥交换算法，简称DH，只是一些流程不同，不深究。


ECDH可以拆分为：EC和DH，
EC的含义：


* elliptic curves——椭圆曲线，从名字就能看出，底层原理类似ECC，


DH的含义


* Diffie–Hellman——是两位数学牛人的名称，他们发明了这个算法，好像也能代指密钥交换。


ECDH的定义：


* ECDH全称是椭圆曲线迪菲\-赫尔曼秘钥交换（Elliptic Curve Diffie–Hellman key Exchange），主要是用来在一个不安全的通道中协商出一把**共享秘钥**，这个共享秘钥一般作为“对称加密”的密钥而被双方在后续数据传输中使用。


我们先来说说 ECDH, 客户端和服务端不传输私钥(需要传输公钥), 就可以计算出一样的结果(共有加密资料), 即使协商过程被第三方(中间人)知晓和监听, 也不会泄露密钥。


而 ECDHE(ECDH Ephemeral) 与 ECDH 无本质差别, 他们协商的流程一模一样, 只是ECDHE代表协商出的共有加密资料是临时的, 就算当前的加密资料泄露, 也不会影响其之前的历史数据被解密, 这是使用方式决定的, 大白话意思就是, 我们通过 ECDH 生成的共有加密数据有实效性, 会通过逻辑在一段时间或特定事件后重新协商, 而不是只协商一次, 如果只协商一次, 如果共有加密资料被泄露, 则之前的所有数据都可以解开。 这种共有加密数据资料泄露也不会对历史数据有影响的特性在密码学中被称为 **前向安全性**。



```
这种共有加密数据资料泄露也不会对历史数据有影响的特性在密码学中被称为 前向安全性——"forward secrecy"。

```

这个多出来的E的意思是指每次公钥都随机生成。因为像HTTPS里那种是可以从证书文件里取静态公钥的。可以理解ECDHE是ECDH的升级版。


# Flow chart


我们可以仿造 TLS1\.2 协议来打造一个前后端通信加密的流程, 但是需要注意以下几点:


* ECDH 本身的协商过程是"明文的", 协商出共享加密数据后使用该数据对 body 进行加密传输才是"安全的"；
* ECDH 变成 ECDHE 是加了时效性, 因此共享加密数据的淘汰策略很重要；
* ECC 生成的公私钥实际上是 XY 坐标, 考虑前端 JS 出问题生成 XY 重复的可能；


修改后的密钥协商流程如下:
![image](https://img2024.cnblogs.com/blog/1552062/202412/1552062-20241204110614329-204238976.png)


之后的请求, 均使用 **aesKey1** 进行 AES256\-CBC 加解密通信，
![image](https://img2024.cnblogs.com/blog/1552062/202412/1552062-20241204110651572-2093798618.png)


# Reference


浅尝 ECDHE 协议流程
[https://github.com/chnmig/p/16833780\.html](https://github.com)


[https://cloud.tencent.com/developer/article/1173441](https://github.com)


