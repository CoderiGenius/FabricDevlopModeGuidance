# Fabric 1.1开发模式使用教程
> 本教程旨在帮助使用者了解Fabric 1.1开发模式使用，实验环境为Centos7.4，go语言版本为1.8 fabric版本为1.1
### 1、	相关路径：
#### Goroot：/root/go
#### Gobin：/root/go/bin
#### Gopath: /opt/gopath
#### Fabric源码：/opt/gopath/src/github.com/hyperledger/fabric
#### Fabric开发模式相关文件：/opt/gopath/src/github.com/hyperledger/fabric-****
### 2、	使用方法：
> 我们这里假设我们编写的智能合约就是官方提供的示例合约chaincode_example02。该智能合约模拟了一个银行，其中含有a和b两个账户，并模拟执行转账操作
#### （1）	进入 /opt/gopath/src/github.com/hyperledger/fabric-****/chaincode/ 目录 
#### （2）	运行service docker start 后执行 ./start.sh up
#### （3）	等待执行ok，没有报错后
#### （4）	进入 /opt/gopath/src/github.com/hyperledger/fabric/example/chaincode/go/chaincode_example02/ 文件夹 （注，这里只是举例，假设智能合约为chaincode_example02，自己写的智能合约请到该文件夹中新建，然后执行下一步）
#### （5）	执行 go build 并执行 ls  查看文件名为 chaincode_example02 的二进制文件是否正常生成 
#### （6）	在当前目录下执行以下命令：
``` 
CORE_CHAINCODE_LOGLEVEL=debug CORE_PEER_ADDRESS=127.0.0.1:7052 CORE_CHAINCODE_ID_NAME=mycc:0 ./chaincode_example02
```
其中 mycc 为chaincode名称
#### （7）  保持当前shell不要关闭，即保持其运行。新开一个shell，进入 /opt/gopath/src/github.com/hyperledger/fabric-****/chaincode/ 目录执行如下命令：
```
FABRIC_CFG_PATH=./sampleconfig peer chaincode install -n mycc -v 0 -p github.com/hyperledger/fabric/examples/chaincode/go/chaincode_example02
```
此举为安装链码
#### (8)    若无报错则继续执行：
```
 FABRIC_CFG_PATH=./sampleconfig peer chaincode instantiate -n mycc -v 0 -c '{"Args":["init","a","100","b","200"]}' -o 127.0.0.1:7050 -C test1
```
此举为初始化链码，对应智能合约中的init方法，相关参数随之在后，可以看到执行成功的提示。
#### （9）若无报错则继续执行：
```
FABRIC_CFG_PATH=./sampleconfig peer chaincode invoke -n mycc -c '{"Args":["invoke","a","b","10"]}' -o 127.0.0.1:7050 -C test1
```
此举为执行链码，对应智能合约中的invoke方法，相关参数随之在后，可以看到执行结果。
#### （10） 若无报错则继续执行：
```
FABRIC_CFG_PATH=./sampleconfig peer chaincode query -n mycc -c '{"Args":["query","b"]}' -o 127.0.0.1:7050 -C test1
```
此举为执行查询，建议写智能合约时查询都使用query来做，可以看到查询结果，query result：200


### 3、开始开发
#### 请使用go语言来编写智能合约，并在  /opt/gopath/src/github.com/hyperledger/fabric/example/chaincode/go/ 路径下新建一个自己命名的文件夹，把自己写的智能合约放到这个文件夹下，按照 2 里面的步骤执行。
 **注意 2 中各小项的参数需要自己根据情况修改！**
 
 
 

---

> 附docker清理命令
每次运行结束区块链网络需要重新部署运行时，需要清理之前的docker容器，不然会启动报错。
这里附上docker删除命令

## 停止所有容器
```
docker stop $(docker ps -a -q)
```
## 删除所有容器
```
docker rm $(docker ps -a -q)
```