# 4. 环境准备
## 4.1 介绍
本章会介绍如何准备本教程所需的软件环境。

在开始后续的Iroha，Sawtooth和Fabric章节之前，你必须安装以下软件：**cURL**，**Node.js**，**npm**包管理器，**Go**语言，**Docker**和**Docker Compose**，如果你是Windows用户，还需要**VirtualBox**。

如果上述软件都安装好了，那么也可以跳过本章。

## 4.2 学习目标
完成本章学习后，你将可以：
- 安装cURL
- 安装Node.js和npm包管理器
- 安装GO语言
- 安装Docker和Docker Compose
- 安装VirtualBox （如果你是Windows用户）

## 4.3 安装cURL
打开终端窗口：
```
Ctrl + Alt + T
```

输入如下命令并输入密码：
```
$ sudo apt install curl
```

运行以下命令检查安装结果：
```
$ curl -V
```

**注意：**“V”要大写

## 4.4 安装Docker
Docker的安装有非常好的[教程](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)

后续的指导假定你的发布版本是**64-bit Ubuntu 16.04 VPS**，因为这是安装Docker最方便的版本。

如果使用Ubuntu，你需要选择是使用社区版（CE）还是企业版（EE）。我们建议使用CE，因为这个版本对于开发者和小团队尝试使用Docker都是极好的。

### 4.4.1 非root用户管理Docker
如果你想要使用**docker**，但是不想**sudo**，那么创建一个Unix组，命名为**Docker**，然后往这个组里添加用户。当**docker**守护进程启动后，它会将Unix套接字 read/writable的所有权交给**docker**组。

**警告：** **docker**组跟**root**的用户的权限是一样的。这对于系统安全性的影响，请参考[Docker守护进程攻击面](https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface)。

创建**docker**组并添加用户的步骤：
1. 创建docker组
```
$ sudo groupadd docker
```
2. 向docker组添加用户
```
$ sudo usermod -aG docker $USER
```
3. 重新登录，这样会重新计算组成员
4. 在桌面Linux环境，比如X windows，彻底登出当前会话，然后重新登录。
5. 不使用sudo验证docker是否可以运行
```
$ docker run hello-world
```
6. 这个命令会下载一个测试镜像，然后在容器中运行它。当容器运行时，它会打印一个信息然后退出。

### 4.4.2 Docker Compose
通过下面命令安装Docker Compose：
```
$ sudo apt update
$ sudo apt install docker-compose
```
检查Docker版本必须是17.03.1-ce或者更新的，Docker Compose的版本是1.9.0或者更新的。
```
$ docker --version && docker-compose --version
```
## 4.5 安装Node.js和npm
通过以下命令安装**Node.js**和**npm**：
```
$ sudo bash -c "cat >/etc/apt/sources.list.d/nodesource.list" <<EOL
deb https://deb.nodesource.com/node_6.x xenial main
deb-src https://deb.nodesource.com/node_6.x xenial main
EOL

$ curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | sudo apt-key add -

$ sudo apt update

$ sudo apt install nodejs

$ sudo apt install npm
```
验证安装结果和版本信息，确保**Node.js**的版本高于v6.9（但不要使用v7），**npm**的版本高于3.x：
```
node --version && npm --version
```
## 4.6 安装Go语言
访问 https://golang.org/dl/ 记录下来最新的稳定版本（1.8或者更新的版本）。
通过以下命令安装Go语言：
```
$ sudo apt update

$ sudo curl -O https://storage.googleapis.com/golang/go1.9.2.linux-amd64.tar.gz
```
**注意：**上述curl地址的最后文件名，需要替换成目标版本的文件名。

```
$ sudo tar -xvf go1.9.2.linux-amd64.tar.gz

$ sudo mv go /usr/local

$ echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile

$ source ~/.profile
```
检查Go语言的版本要大于等于1.8版：
```
$ go version
```

## 4.7 准备就绪？
你的Linux系统到此为止已经为后面的3章的超级账本框架做好了准备。你可以跳过本章后续内容直接开始[第5章 超级账本Iroha项目入门](chapter5_hyperledger_iroha.md)。

## 4.8 Mac环境准备
略

## 4.9 Windows环境准备
略

- [**目录**](README.md)
- [**上一章 区块链技术前景**](chapter3_blockchain_rise.md)
- [**下一章 超级账本Iroha项目入门**](chapter5_hyperledger_iroha.md)
