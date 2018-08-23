---
layout: post
title:  "Docker常用命令"
date:   2018-08-24 00:00:00 +0800
categories: docker
---

# 镜像
```shell
# 列出本地所有镜像
$ docker images

# 删除镜像
$ docker rmi -f [image-name]

# 查看镜像的提交记录
$ docker history [image-name]

# 查找镜像
$ docker search [image-name]

# 构建镜像
$ docker build
```

# 容器

# 镜像仓库
```shell
# 登录镜像仓库
$ docker login [docker-registry-host]

# 退出镜像仓库
$ docker logout

# 将本地镜像提交到远程镜像仓库
$ docker push

# 从远程镜像仓库获取镜像
$ docker pull
```