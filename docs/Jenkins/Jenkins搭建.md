### 一、Jenkins是什么？

Jenkins是一个开源的、提供友好操作界面的持续集成(CI)工具，起源于Hudson（Hudson是商用的），主要用于持续、自动的构建/测试软件项目、监控外部任务的运行（这个比较抽象，暂且写上，不做解释）。Jenkins用Java语言编写，可在Tomcat等流行的servlet容器中运行，也可独立运行。通常与版本管理工具(SCM)、构建工具结合使用。常用的版本控制工具有SVN、GIT，构建工具有Maven、Ant、Gradle。

### 二、CI/CD是什么？

CI(Continuous integration，中文意思是持续集成)是一种软件开发时间。持续集成强调开发人员提交了新代码之后，立刻进行构建、（单元）测试。根据测试结果，我们可以确定新代码和原有代码能否正确地集成在一起。借用网络图片对CI加以理解。

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2BmibOOxjXPvcNJ0IDKlicH10HakUOmYYHbMiaib8MOMv1LIQFerzMO61Kw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

CI

CD(Continuous Delivery， 中文意思持续交付)是在持续集成的基础上，将集成后的代码部署到更贴近真实运行环境(类生产环境)中。比如，我们完成单元测试后，可以把代码部署到连接数据库的Staging环境中更多的测试。如果代码没有问题，可以继续手动部署到生产环境。下图反应的是CI/CD 的大概工作模式。

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2SgPGjnbRiaXqgO20bibEScBmlHerc9MtTRuVYfL2TBTOxdYJ3va8HNXA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

CI/CD

### 三、Jenkins搭建

> 操作系统版本：CentOS Linux release 7.2.1511 (Core)

本次需要安装的软件列表：

- JDK
- Maven
- Jenkins
- Git
- GitLab

#### 1、JDK

参考：Linux 安装 Java

#### 2、Maven

参考：Linux 安装 Maven

#### 3、Git

开始安装git所需要的依赖包

```
yum -y install curl-devel gettext-devel expat-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker
```

下载Git源码包

```
cd /usr/local/git
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
tar -zxvf  git-2.9.5.tar.gz
cd git-2.9.5
./configure --prefix=/usr/local
make
sudo make install
git --version
```

#### 4、Jenkins

官网地址：https://pkg.jenkins.io/redhat-stable/

**安装**

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum install jenkins

```

**配置文件**

配置文件在 /etc/sysconfig/jenkins，可以修改端口、jdk:

```
sudo vim /etc/sysconfig/jenkins
## JENKINS_PORT="8080" 
## JENKINS_JAVA_CMD="/usr/local/java/jdk1.8.0_131/bin/java"
```

**启动**



```
sudo service jenkins start
## sudo service jenkins start/stop/restart/status
```

**访问**

访问Jenkins，进行初始化操作，地址：http://[ip]:[port]

初始密码在：/var/lib/jenkins/secrets/initialAdminPassword

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2gGB2kbulyyYk5y52XibBOmFO9JbiahTudz4LxDLycpjk5Fia19zjNQUmw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

初始密码

选择“Install suggested plugins”安装默认的插件，下面Jenkins就会自己去下载相关的插件进行安装。

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2S304FzeoLB2jgxKcBpWiaCcmtlNv2Lu0b9d9ZVY6qexiazoI0Yux29oA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装插件1

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2KiaCxKZ7MWN2zM93qeBOk1h4TDOSKefv4K2fJzFhg83CJnsI278DVNw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

正在安装

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2W1m9BrcwdFVCPOiasNBah4I2GajvW4lI1SEWs20XbSqNp3eYwq0vTPg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

设置管理员账号密码

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2ynmBLI3ibWz96w7GiaBLHwjfdILUZ51PwyPCYoC7808MW1WLnOelClhA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

web访问地址

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2iczUdEO6wSLAib85FvjGyib2ib8uflJZczwWTjLMs1NfjxkYJzrsIXIbFg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

开始使用

输入账号密码登录。

**安装插件**

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2YGfQGZUZaldwpz78MCvbbhFpP2dkgQMPSlTkTLMInXftvJuCMTf7icw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

插件管理

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2KOrfA2Xs9zg7rCscvRvmJtnyhpZY65w7R78ubzuCLhpfpRcUnV3HkA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

可选插件

安装SSH、Publish Over SSH，用于ssh连接的：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2CHjGEEeyxicMgNSDE2VY2LB1yQTL8v3KOYP9mls39H6GWCmibLL9ugaA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

ssh连接

安装GitLab为git客户端从git主库取代码：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2NeDJcFuCf4HAJTgLdiaVE3hms8zHImCKKI8WEmhmrSA6Btq8ZbAO52w/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

gitlab插件

安装blue ocean插件，是一个界面显示插件：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2F1cY6k6cGcErRDWLQDUfLYvkdiaCy94tdykBHTuHHeo88rXd06mTm5w/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

界面显示插件

安装Maven Integration plugin 编译打包：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2WCKiblEia7He7ynSreUXOueGT3dTCcTezKtU3zSXXaiaZdLRbSbW0Pzqw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

2121.jpg

重启后登录Jenkins

创建Job

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2dzHbtzF9KNbTKiaGJ7a0FuaVwJVicjaN0gtzlkiaZGAuMq3uXYIZ6t3jA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

创建Job

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2UqarF1SZobvYepLJAeGjicxfK1rNnricXmoWUOnaaAVAJO4QC1jwTMRg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

源码管理

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX23ws81teUn7lIP93UApllbINqxjibFq5CmOlTBPCvSS1QpjmUAbibvnqA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

凭证

认证类型可选：
Username with password : 填写用户名密码。
SSH Username with private key : 需要生成公私钥，执行ssh-keygen -t rsa后，连续三个回车，会在用户根目录下生成id_rsa私钥和id_rsa.pub公钥，分别到Jenkins Web（私钥）和Gitlab Web（公钥）配置。

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2iceLuqbIKpL2FXb4ZhMo3T9oH5vVCDiaUiaEQWTzuQLReiagH34GOLEibyQ/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

生成公私钥

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2ymCSZ7dawBrWass3icybjwKiaSbibicLxenJmnhficMLbwlic5fOJ2mprxwQ/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

ssh文件

拉取代码，到本地：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2sHKSu1Ougf675CrrqbCialrEicnjgbFxG8vfV4ZB4YqI5wRozCkYrVKg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

拉取代码

**拉取失败问题**

1、拉取超时问题？

通过配置“源码管理-git-Additional Behaviours”中的Advanced clone behaviours：

Shallow clone：勾选后不获取历史版本；

Timeout (in minutes) for clone and fetch operation：配置后覆盖默认的超时时限。

#### 5、Jenkins 节点

**什么是Jenkins节点？**

节点是Jenkins任务执行的具体环境，通常在安装Jenkins这台服务器默认就是一个主节点（俗称master），其他相对于这台安装Jenkins的机器都称为从节点（俗称slaves）。

**为什么要配置节点？**

同一时间需要多台机器来执行Jenkins任务，比如需要将产品部署到100台服务器，那么这100台服务器必须纳入到Jenkins管理的节点里才可以通过Jenkins管理；

不同的Jenkins任务有不同的操作环境需求，比如部署基于IIS服务的需要windows操作系统，构建IOS应用需要MacOs，构建脚本是shell的需要Linux操作系统；

所以为了满足任务执行需求，需要准备不同操作系统的节点。

**配置节点**

> 以添加windows为例

a.准备一台windows服务器，安装jdk;

b.登录Jenkins管理端，进入[系统管理>节点管理>新建节点];

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2Inh1PtwpSvLIx0o4WyEy3ER7wic31ial6yZtNcRVxr9GPuqUfsaQTQRw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

新建节点

c.配置节点信息，主要是红框中的设置，标签设置的内容后面要用到；

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2xic4rhtyuMNcW6XXZnhxkt41teG8ukJk3AQzcVNgMcAkEibJYXepxU1A/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

节点信息

保存后返回节点列表，X号表示刚才添加的节点未运行；

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2TaQkvZwo308qhFzqFuhnYwx3xSp2kWd5UYstiaD2RTXUToOV53204Qg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

节点列表

d.点击刚才添加的节点，提供了两种方式运行节点；

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2gib4GTQ0O1iaEJS3VicTy9yaRdicBJBmysFGBn4gNrNRJYRLC8J19ER9bA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

启动节点

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2xibVZ8YMFkeOptyNfic84k5icFF8dficDzsBBt1sWSHSPpnpUndlcaI0mw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

第一种方式

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2PGBn6WyoZvtDiatU6OIBwPnCWM9ImgQQFM3jMh9lPbIZegrq3BDgS6w/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

第二种方式

e.创建自由风格的JOB，选择“限制项目的运行节点”；

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2HsqKp2zmP7SQrDvicFI0KwHqicsEGq4B4bib8QoJS4VyO2AWiaic04zHxNA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

选择节点

在【构建】中执行windows脚本：

![img](https://mmbiz.qpic.cn/mmbiz/QvRgo8QLV9MbZSTdaQvOl1EODVT6XsX2aG0Uwvfa4vgPTC2IjMu0KTS6fGow3TDxlCulGO9fnibwiaZiaibMoQyZew/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

windows脚本