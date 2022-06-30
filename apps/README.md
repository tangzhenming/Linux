# apps

## [在 CentOS 上安装 docker](https://docs.docker.com/engine/install/centos/#installation-methods)

设置镜像源

```shell
# 该依赖用于使用 yum-config-manager 命令来设置镜像源
yum install -y yum-utils

# 官方
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 阿里云
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

安装 Docker engine

```shell
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

Start Docker.

`sudo systemctl start docker`

Verify that Docker Engine is installed correctly by running the hello-world image.

`sudo docker run hello-world`