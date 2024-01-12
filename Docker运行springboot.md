# 安装Docker

## 删除残留Docker文件

```sh
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

## 安装工具

```sh
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

设置aliyun仓库地址

```sh
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

查看Docker版本

```sh
yum list docker-ce --showduplicates | sort -r
```

## 安装Docker

```sh
sudo yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
# 或者
sudo yum install docker-ce docker-ce-cli
```

启动Docker

```sh
sudo systemctl start docker
```

设置开机启动

```sh
sudo systemctl enable docker
```

查看版本

```sh
docker -v
```

## 测试安装是否成功

```sh
docker run hello-world
```

如果安装成功

```sh
docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:62af9efd515a25f84961b70f973a798d2eca956b1b2b026d0a4a63a3b0b6a3f2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 如果测试运行错误执行

修改配置文件

```sh
sudo tee -a /etc/sysctl.conf <<-EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```

重新加载配置文件

```sh
sudo sysctl -p
```

```sh
FROM java:8

MAINTAINER eangulee <eangulee@gmail.com>

VOLUME /tmp 

ADD demo-0.0.1-SNAPSHOT.jar app.jar 

RUN bash -c 'touch /app.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

