防止重放攻击底层代码开发
=====================================

https://github.com/btcgroup2/bitcoin

什么是重放攻击
----------------
重放攻击(Replay Attacks)又称重播攻击、回放攻击，是指攻击者发送一个目的主机已接收过的包，来达到欺骗系统的目的，主要用于身份认证过程，破坏认证的正确性。重放攻击可以由发起者，也可以由拦截并重发该数据的敌方进行。攻击者利用网络监听或者其他方式盗取认证凭据，之后再把它重新发给认证服务器。重放攻击在任何网络通过程中都可能发生，是计算机世界黑客常用的攻击方式之一。

区块链重放攻击防止
----------------
本项目主要模拟区块链系统在硬分叉以后，如何防止原来区块链上的交易同步到新的区块链上。通过，改变签名的算法，达到防止重放攻击的目的。
通过清华大学icenter老师们的悉心教导，通过将近2个月的学习，在比特币源码的基础上完成了防重放攻击1.0的开发。

license
-------

MIT license. 欢迎大家多多交流
具体请参考 https://opensource.org/licenses/MIT.

开发流程
-------------------

。。。

测试流程
-------

节点准备
一个比特币旧节点(node1)
一个2k区块节点（为了方便测试，没有做2m的节点）(node2)
一个能够防重放攻击的2k区块节点(node3)

测试过程
node1 node2 node3先都安装比特币官方代码编译的程序
任意节点发起交易，生成区块，保证区块被确认(后继生成6个以上区块)
然后node2更新成2k区块的节点，node3更新成能防重放攻击的2k区块节点

node1发起多笔交易生成区块（保证区块大小超过2k）,此区块测试结果不能在node2和node3接受，由此区块产生了分叉。
下一步node1再发起交易，node2生成区块，测试结果，交易被包含到区块中，证明node2可被攻击
此区块被node2发布到网络，node3不接受此区块，证明node3可以阻止重放攻击。


docker技术的使用
-------
为了解决多节点的测试问题，采用当前流行的docke技术。
著名的Hyperledger的fabric项目(由IBM主导的)，就用了容器技术进行网络隔离。
本次开发虚拟化了6个节点(2个bitcoin的源代码节点，2个硬分叉节点和2个防攻击节点)

#### 1.所有镜像都上传到了docker hub 上，大家可以随时下载，下载命令如下:
```
docker pull xuxinlai2002/btcnew
docker pull xuxinlai2002/btchardfork 
docker pull xuxinlai2002/btcorg
```
![d1](https://github.com/btcgroup2/bitcoin/blob/master/share/docker_hub.png)

#### 2.可以通过以下命令查看所有镜像:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
xuxinlai2002/btcorg        latest              36947581fdb1        44 hours ago        2.73 GB
xuxinlai2002/btcnew        latest              47701325c0b2        2 days ago          5.872 GB
xuxinlai2002/btchardfork   latest              06a5c890dda1        2 days ago          7.86 GB
```

#### 3.可以通过以下命令查看所有容器:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# docker ps
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS              PORTS               NAMES
da5c88d83f3e        xuxinlai2002/btcnew        "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btcnewNode2
162954e0feac        xuxinlai2002/btcnew        "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btcnewNode1
2907e68ad502        xuxinlai2002/btchardfork   "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btchardforkNode2
ab2ba80bff53        xuxinlai2002/btchardfork   "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btchardforkNode1
84955fb307e8        xuxinlai2002/btcorg        "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btcorgNode2
ed2d1223c275        xuxinlai2002/btcorg        "sh -c 'bitcoind -reg"   10 minutes ago      Up 10 minutes       18444/tcp           btcorgNode1
```
#### 4.在工作目录，可以通过以下命令清空上次的执行结果:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# cleanAll
da5c88d83f3e
162954e0feac
2907e68ad502
ab2ba80bff53
84955fb307e8
ed2d1223c275
```

#### 5.在工作目录，可以通过以下命令自动配置docker环境:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# setAll
9581b304488f2d2b042d5f39b038e37a8666625c46eae02e0019ba7689d9fa01
7c476428e7807267ab169f32c4e57f73bf39e6506aa71453954c2afe00c30857
b1323bd91291ca7b8cf7057b32c72029af17f04bf0036c3bc5868ba096fdb443
1d35b593c37281677d3723f51eb08e6b72b7cba2eb304223a0f822c8578e8a39
2124409a8df4a52eddd1c7acc2a38251e2cd0701d7ee4e4eb725a8902feed141
e74fa0d9398c7d7732dc1726d45445737ac7b0d6f2cb2010b775c65846a90c74
```

#### 6.在工作目录，可以通过以下命令重置环境(docker重启，数据归零):
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# resetAll
/root/mydocker
e74fa0d9398c
2124409a8df4
1d35b593c372
b1323bd91291
7c476428e780
9581b304488f
e2cbc3d893942329ccdeb514eb97d116838a297443681b5c5f3cb11730c8181d
d95f254c75686e086c6d45827438c346e5166434704e6e2f51dbd4c40462ba21
58df1c5e832aac2463f3d6a28e81865677eac6986fe9c91b8812893c3c84bf67
01ae29ac4832b87fed5d865e38c57704708f98fa170eac5b93ba4ac0a0379d65
076938e94aca1109dc690639f3775b74a6def6a6edda092b4d666ccfcbaf7e5f
272e228f0a94e474595e1f789d118624cfb1c86cb35ad70e1c215719acc4ba42
```

#### 7.以下目录是所有docker执行，所需要的自动化脚本:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker/tools# ll
total 24
drwxr-xr-x  2 root root 4096 Jul 10 21:42 ./
drwxr-xr-x 10 root root 4096 Jul 10 21:04 ../
-rwxrwxrwx  1 root root  328 Jul 10 16:43 cleanAll*
-rwxrwxrwx  1 root root   31 Jul 10 15:34 resetAll*
-rwxrwxrwx  1 root root  294 Jul 10 16:37 setAll*
-rwxrwxrwx  1 root root  705 Jul  9 23:28 startNodes*
```

