DOCKER

[toc]



## Windows

### Win10家庭普通版安装

1. read官网,	找到Docker Toolbox下载地址,	安装时根据自己是否有git选择安装 (我的有)

2. 修改Docker Quickstart Terminal启动属性`D:\Applications\Git\bin\bash.exe --login -i "d:\Program Files\Docker Toolbox\start.sh"`

3.  将Docker Toolbox文件夹下的boot2docker.iso复制到C:\Users\john\.docker\machine\cache下断网,

   启动Docker Quickstart Terminal

4. ok

### Vagrant安装



## 容器

docker,	kubernetes<k8s>

简化配置,	整合服务器,	代码流水化管理,	调试能力,	隔离应用,	快速部署,	多租户

DevOps	=	文化+过程+工具



原始:	物理服务器---操作系统---app

虚拟化技术:	物理服务器---主机操作系统---虚拟化---多个操作系统----分别安装应用---云化

容器:	物理服务器---操作系统---docker共享底层kernel---创建不同容器---容器内安装每个应用,	依赖Bins/Libs

虚拟化+容器:	物理服务器---操作系统---虚拟化---多个操作系统----多个docker----每个docker继续创建容器---应用





bash

~~~bash
c:/user
mkdir Vagrant
cd Vagrant
mkdir centos7
cd centos7

vagrant init centos/7
vagrant up[下载过慢]
[如果执行过上次两步, 需要删除centos7下文件]
vagrant box add centos/7 E:\virtualBox\CentOS-7-x86_64-Vagrant-1905_01.VirtualBox.box
vagrant init centos/7
vagrant up centos/7[如果启动不了可以在powershell运行]

vagrant ssh[这样就可以使用linux的命令]
[删除旧版本docker]
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
                  
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    

~~~







