防止重放攻击底层代码开发
=====================================

https://github.com/btcgroup2/bitcoin

什么是重放攻击
----------------

。。。。

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

#### 2.通过可以以下命令查看所有镜像:
```
root@iZ2ze4wxzv9g5i5r69vu06Z:~/mydocker# docker images
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
xuxinlai2002/btcorg        latest              36947581fdb1        44 hours ago        2.73 GB
xuxinlai2002/btcnew        latest              47701325c0b2        2 days ago          5.872 GB
xuxinlai2002/btchardfork   latest              06a5c890dda1        2 days ago          7.86 GB
```

#### 3.通过可以以下命令查看所有容器:
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

#### 4.在工作目录，通过可以一下命令重置环境(docker重启，数据归零):
>resetAll
