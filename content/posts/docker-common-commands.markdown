---
title: "Docker常用命令"
date: 2018-08-24T00:00:00+08:00
draft: false
---

# 镜像
```shell
# 列出本地所有镜像
$ docker images
$ docker image ls -a

# 删除镜像
$ docker image rm <image-name>

# 查看镜像的提交记录
$ docker history <image-name>

# 查找镜像
$ docker search <image-name>

# 构建镜像
$ docker build -t <image-name> .

# 运行镜像
$ docker run -p <local-port>:<container-port> <image-name>

# 以分离模式运行镜像
$ docker run -d -p <local-port>:<container-port> friendlyname
```

# 容器
```shell
# 列出所有正在运行的容器
$ docker container ls

# 列出所有容器
$ docker container ls -a

# 启动容器
$ docker container start <container-id>

# 停止容器
$ docker container stop <container-id>

# 强制停止容器
$ docker container kill <container-id>

# 删除容器
$ docker container rm <container-id>
```

# 镜像仓库
```shell
# 登录镜像仓库
$ docker login <docker-registry-host>

# 退出镜像仓库
$ docker logout

# 打标签
$ docker tag <image-name> username/repository:tag

# 将本地镜像提交到远程镜像仓库
$ docker push username/repository:tag

# 从远程镜像仓库获取镜像
$ docker pull username/repository:tag

# 运行仓库中的一个镜像
$ docker run username/repository:tag
```