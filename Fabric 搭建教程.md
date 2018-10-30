# Fabric 搭建教程
## 本教程适合有运维基础的人员读阅
### Fabric所需要的基础环境：
  - go语言
  - docker
  - docker-compose
### 开始安装
1. 安装go语言
    1. 下载Golang

    ```
        [root@VM_0_14_centos golang]# wget https://storage.googleapis.com/golang/go1.8.1.linux-amd64.tar.gz
    ```
    2. 解压，然后移动到指定目录

    ```
    [root@VM_0_14_centos golang]# tar -zxvf go1.8.1.linux-amd64.tar.gz 
    [root@VM_0_14_centos golang]# mv go /usr/local/
    ```
    
    3.添加到环境变量

    ```
    [root@VM_0_14_centos golang]# vim /etc/profile

    修改内容为(在path后添加)
    export GOROOT=/usr/local/go
    export GOBIN=$GOROOT/bin
    export PATH=$PATH:$GOBIN
    export GOPATH=/opt/gopath
    ```
    > 解释 GOROOT为go语言的安装目录，请修改环境变量的时候确定是否是go语言的安装目录
    
    > 解释 GOPATH为fabric的工作目录，一定要把源码下载到这个目录
    
2. 安装docker
    1. 卸载已有的docker CE
    ```
    sudo yum remove docker \ 
                               docker-common \ 
                               docker-selinux \ 
                               docker-engine
    ```
    2. 开始安装Docker ce
    ```
    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum-config-manager --enable docker-ce-edge
    sudo yum-config-manager --enable docker-ce-test
    sudo yum-config-manager --disable docker-ce-edge
    sudo yum makecache fast
    sudo yum install docker-ce

    ```
    3. 检查是否安装成功
    ```
     docker --version
    ```
    4. 启动docker
    ```
    service docker start
    ```
3. docker-compose 安装
    1. 需要用到服务器的curl功能, 因此需要安装curl
    ```
    yum install curl
    ```
    2. 安装docker-compose
    ```
    curl -L https://github.com/docker/compose/releases/download/1.15.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    ```
    3. 授权
    ```
    chmod +x /usr/local/bin/docker-compose
    ```
    4. 查看是否安装正确，以及版本
    ```
    docker-compose --version
    ```
    > docker常用命令 
    
    ```
    杀死所有正在运行的容器
    docker kill $(docker ps -a -q)

    删除所有已经停止的容器
    docker rm $(docker ps -a -q)

    删除所有镜像
    docker rmi $(docker images -q)

    强制删除所有镜像
    docker rmi -f $(docker images -q)
    ```
4. 下载fabric源码
    1. 首先切换到工作目录
    ```
    cd /opt/lcoal/gopath
    ```
    2. 新建文件夹
    ```
    mkdir -p src/github.com/hyperledger
    ```
    3. 然后进入到如下路径
    ```
    cd /opt/gopath/src/github.com/hyperledger
    ```
    4. 下载fabric源码
    ```
    git clone https://github.com/hyperledger/fabric.git
    ```
    5. 进入fabric目录查看fabric的git版本
    ```
    cd fabric/
    git branch -a
    ```
    6. 可以切换分支，这里咱们使用1.1分支
    ```
    git checkout release-1.1
    ```
    
5. 下载fabric-dev
    1. 切换到
    ```
    cd /opt/gopath/src/github.com/hyperledger
    ```
    2. 下载源码
    ```
    git clone https://github.com/luckydogchina/fabric-v1.1.0-chaincodedev.git
    ```
    3. 切换到
    ```
    cd chaincodedev
    ```
    4. 执行
    ```
    sudo ./start.sh
    ```
    > 注意，要保持网络连接，会先下载镜像，再启动fabric开发模式
    
    > 之后则按照 https://github.com/CoderiGenius/FabricDevlopModeGuidance/blob/master/FabricDevlopModeGuidance.md 进行即可
    