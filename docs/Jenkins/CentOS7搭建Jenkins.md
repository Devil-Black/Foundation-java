# Jenkins 是什么

Jenkins 是什么嘞？它提供了软件开发的持续集成服务，运行在 Servlet 容器中(例如 Apache Tomcat )。它支持软件配置管理( SCM )工具(包括 AccuRev SCM、CVS、Subversion、Git、Perforce、Clearcase 和 RTC )，可以执行基于 Apache Ant 和 Apache Maven 的项目，以及任意的 Shell 脚本和 Windows 批处理命令

有点儿晦涩难懂？那就记住一句话就可以了， Jenkins 的存在是为了简化我们的开发流程，比如我们往 git 上提交了代码， Jenkins 就会自动拉取最新的代码帮我们部署

# CentOS7 下 Jenkins 搭建过程

Jenkins 需要 jdk 环境，阿粉这里就不做示范了

安装完 jdk 环境之后，就可以准备安装 Jenkins ，几条命令即可（#后面为注释内容）：

```shell
#下载Jenkins库
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo 

#导入key
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

#安装Jenkins
yum install -y jenkins

#启动Jenkins服务
systemctl start jenkins 
```

到这里， CentOS 下 Jenkins 搭建便是完成了

此时我们可以通过 ip:port 的方式，访问到 Jenkins ，如下图所示：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/laEmibHFxFw6CtXBia5pmnfSeR5KMOfpN8eod4qTm3of2mtcvk0TaUQsHlyQS2tbfKQ1iaEKOH9K6R5JJQEnMuPibA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

点击Continue之后，会出现下图：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/laEmibHFxFw6CtXBia5pmnfSeR5KMOfpN85Sia8CeCE0P5Lb3849BVqf9PCMbRKexdibdL6rYVtdez5bIY89tQJ2Mw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后根据自己的需求，进行安装即可。到此， CentOS 下 Jenkins 搭建便是完成了

是不是还挺简单~

# 可能遇到的问题

1 ，Starting jenkins (via systemctl): Job for jenkins.service failed because the control process exited with error code.

运行命令: `systemctl status jenkins.service` 查看错误详细信息

![img](https://mmbiz.qpic.cn/mmbiz_jpg/laEmibHFxFw6CtXBia5pmnfSeR5KMOfpN86PKQ4OVOhp5K0Xw5unafcVX42wWXtaexIQS4Iy3iagfK3xTmiceysOQw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

请注意我在图中标注出来的两个地方，第一个地方是 Jenkins 加载的路径，第二个地方是 Jenkins 的错误详细信息： `Failed to start LSB: Jenkins Automation Server`

这是因为 Jenkins 未加载到 java 环境的问题，直接修改 Jenkins 的启动文件，并在 candiddates 参数内追加 java 的环境变量即可

Jenkins的启动文件，在图中第一个地方我已经做了标注，所以运行以下命令：`vi /etc/rc.d/init.d/jenkins`

具体修改见下图:

![img](https://mmbiz.qpic.cn/mmbiz_jpg/laEmibHFxFw6CtXBia5pmnfSeR5KMOfpN8aF5UVI2pQyGUDWwiawIicnURAr1UaC6PRKeXk8oQz7OD9Im1RcNiaARGA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



关于 java 环境变量这部分，因为每个人的配置不同，所以你需要根据自己的实际情况做修改。修改完之后再做以下步骤即可（#后内容为注释内容）：

```shell
#重新启动Jenkins服务
systemctl restart jenkins.service

#查看Jenkins服务,可以看到服务已经起来了
systemctl status jenkins.service
```

2 ，在 CentOS 环境下， Jenkins 已经安装好了，但是在外部访问不到。这可能是因为防火墙的问题

出于安全的考虑，我是不建议直接将防火墙关掉的。开启 Jenkins 需要的端口即可（以开启 8080 端口为例，具体可根据自己需求更改）：

```shell
开端口命令：firewall-cmd --zone=public --add-port=8080/tcp --permanent
重启防火墙：systemctl restart firewalld.service

命令含义：
 
--zone #作用域
 
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
 
--permanent   #永久生效，没有此参数重启后失效
```

因为 Jenkins 默认端口是 8080 ，可能会导致端口冲突。修改 Jenkins 的默认端口即可： `vi /etc/sysconfig/jenkins`

在该配置文件中，可以看到 JENKINS_PORT 这一项，根据需求修改即可

到这里， Jenkins 就已经没有任何问题的安装上了