1����װ�ر�������� #1-5 ������  +6-10 ˽����
$ yum update -y && yum install git wget bzip2 vim gcc-c++ ntp epel-release nodejs cmake -y
$ yum install -y curl git golang nodejs gcc-c++

2����װgo����
$ wget https://dl.google.com/go/go1.10.linux-amd64.tar.gz
$ tar -C /usr/local -xzf go1.10.linux-amd64.tar.gz
$ echo 'export GOROOT=/usr/local/go' >> /etc/bashrc 
$ echo 'export PATH=$PATH:$GOROOT/bin' >> /etc/bashrc  
$ echo 'export GOPATH=/root/go' >> /etc/bashrc
$ echo 'export PATH=$PATH:$GOPATH/bin' >> /etc/bashrc
$ source /etc/bashrc

3����֤go���԰�װ�ɹ�
[root@jubeiTest root]# go version
go version go1.10 linux/amd64

4����װgeth�ͻ���   '�ҵ�·��'=/mnt/
$ git clone https://github.com/ethereum/go-ethereum.git  
$ cd go-ethereum  
$ make all
$ echo 'export PATH=$PATH:/�����·����/go-ethereum/build/bin' >> /etc/bashrc
$ source /etc/bashrc

5����֤geth
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

6�����ܺ�Լ
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

ͬ���ڵ�
> eth.syncing
{
  currentBlock: 164096,
  highestBlock: 6841125,
  knownStates: 318087,
  pulledStates: 307252,
  startingBlock: 0
}

7�����������ļ���genesis.json��
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

8����ʼ�������ļ�
$ geth --datadir �����·���� init �����·����/genesis.json

9������start.sh�ű�
(nohup geth --syncmode "fast" --cache=512 --datadir "/mnt/geth_data/data0" --rpc --rpcapi db,net,eth,web3,personal --rpcaddr 0.0.0.0 --rpccorsdomain "*" 2>>geth_log &)

10���������̨
$ cd �����·����
$ geth attach ./geth.ipc
ͬ������eth.syncing


history����
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




