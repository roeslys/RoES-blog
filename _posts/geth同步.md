1、安装必备环境插件 #1-5 公有链  +6-10 私有链
$ yum update -y && yum install git wget bzip2 vim gcc-c++ ntp epel-release nodejs cmake -y
$ yum install -y curl git golang nodejs gcc-c++

2、安装go语言
$ wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
$ tar -C /usr/local -xzf go1.10.linux-amd64.tar.gz
$ echo 'export GOROOT=/usr/local/go' >> /etc/bashrc 
$ echo 'export PATH=$PATH:$GOROOT/bin' >> /etc/bashrc  
$ echo 'export GOPATH=/root/go' >> /etc/bashrc
$ echo 'export PATH=$PATH:$GOPATH/bin' >> /etc/bashrc
$ source /etc/bashrc

3、验证go语言安装成功
[root@jubeiTest root]# go version
go version go1.10 linux/amd64

4、安装geth客户端   '我的路径'=/mnt/
$ git clone https://github.com/ethereum/go-ethereum.git  
$ cd go-ethereum  
$ make all
$ echo 'export PATH=$PATH:/’你的路径’/go-ethereum/build/bin' >> /etc/bashrc
$ source /etc/bashrc

5、验证geth
[root@jubeiTest go-ethereum]# geth version
Geth
Version: 1.8.18-unstable
Git Commit: 1ff152f3a43e4adf030ac61eb5d8da345554fc5a
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.10
Operating System: linux
GOPATH=/root/go
GOROOT=/usr/local/go

6、智能合约
$  cd ~
$  wget https://cmake.org/files/v3.9/cmake-3.9.2.tar.gz  
$  tar xvf cmake-3.9.2.tar.gz && cd cmake-3.9.2
$  ./configure && make && make install
$ pwd
$ /home/ddp/cmake-3.9.2

$ cd /mnt/
$ mkdir geth_data
$ touch start.sh
$ chmod 755 start.sh
$ vim start.sh
(nohup geth --syncmode "fast" --cache=512 --datadir "/mnt/geth_data/data0" --rpc --rpcapi db,net,eth,web3,personal --rpcaddr 0.0.0.0 --rpccorsdomain "*" 2>>geth_log &)
$ sh start.sh
$ cd geth_data
$ geth attach ./geth.ipc
> eth.syncing

同步节点
> eth.syncing
{
  currentBlock: 164096,
  highestBlock: 6841125,
  knownStates: 318087,
  pulledStates: 307252,
  startingBlock: 0
}

7、创建创世文件（genesis.json）
{
  "config": {
        "chainId": 15,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    },
  "alloc"      : {},
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000001993",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00"
}

8、初始化创世文件
$ geth --datadir ‘你的路径’ init ‘你的路径’/genesis.json

9、创建start.sh脚本
(nohup geth --syncmode "fast" --cache=512 --datadir "/mnt/geth_data/data0" --rpc --rpcapi db,net,eth,web3,personal --rpcaddr 0.0.0.0 --rpccorsdomain "*" 2>>geth_log &)

10、进入控制台
$ cd ‘你的路径’
$ geth attach ./geth.ipc
同步命令eth.syncing


history命令
     yum update -y && yum install git wget bzip2 vim gcc-c++ ntp epel-release nodejs cmake -y
     wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
     tar -C /usr/local -xzf go1.10.linux-amd64.tar.gz
     echo 'export GOROOT=/usr/local/go' >> /etc/bashrc
     echo 'export PATH=$PATH:$GOROOT/bin' >> /etc/bashrc
     echo 'export GOPATH=/root/go' >> /etc/bashrc
     echo 'export PATH=$PATH:$GOPATH/bin' >> /etc/bashrc
     source /etc/bashrc
     go version
     cd /mnt/
     git clone https://github.com/ethereum/go-ethereum.git
     cd go-ethereum/
     make all
     echo 'export PATH=$PATH:/mnt/go-ethereum/build/bin' >> /etc/bashrc
     source /etc/bashrc
     cd /mnt/
     mkdir
     mkdir geth_data
     cd geth_data/




