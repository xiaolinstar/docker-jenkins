# Jenkins with 「docker in docker」

<!-- PROJECT SHIELDS -->

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]

> 基于docker-compose快速构建Jenkins容器，且Jenkins容器与宿主机docker环境连通，实现「docker in docker」。

## 项目特点

- 使用docker部署Jenkins，与宿主机操作系统隔离，保证系统的隔离性和整洁性
- 轻松支持多版本，不同操作系统环境迁移性（可移植性）强
- Jenkins容器内置docker「docker in docker」，不必在Jenkins内部再安装npm maven go mysql等环境

## 更新日志

2024-09-21：创建项目，首次提交

## 目录

- [前置要求](#前置要求)
- [快速开始](#快速开始)
- [Jenkins快速体验](#jenkins快速体验)
- [References](#references)
- [联系作者](#联系作者)

## 前置要求

1. 默认基础环境：amd64 Linux，推荐使用 Ubuntu LST Version
2. 安装 docker 和 docker compose (V2)

## 快速开始

克隆项目

```bash
git clone https://github.com/xiaolinstar/docker-jenkins.git 
```

进入项目

```bash
cd docker-jenkins
```

检查挂载卷 ，本项目中 `docker-compose.yaml` 中的挂载卷值默认为：

```yaml
volumes:
  - '/usr/bin/docker:/usr/bin/docker'
  - '/var/run/docker.sock:/var/run/docker.sock'
  - './jenkins_home:/var/jenkins_home' 
  - '/usr/libexec/docker/cli-plugins/:/usr/libexec/docker/cli-plugins' # docker 插件挂载
```

👀在 Windows 或 macOS 中下载 Docker Desktop 可能存在参数不一致，请自行检查并酌情修改

```yaml
# Macbook Pro M1pro 
# Docker Desktop
volumes:
  - '/usr/local/bin/docker:/usr/bin/docker'
  - '~/.docker/run/docker.sock:/var/run/docker.sock'
  - './jenkins_home:/var/jenkins_home'
  - '~/.docker/cli-plugins:/usr/libexec/docker/cli-plugins'  
```

启动容器

```bash
# 创建jenkins_home并在后台启动docker容器
mkdir jenkins_home && docker compose up -d 
```

检查容器状态

启动的Jenkins容器名默认为 `xiaolin-jenkins`

```bash
docker ps
```

进入 `xiaolin-jenkins` 容器内部，查看 `docker` 命令

```bash
# 宿主机执行
docker exec -it xiaolin-jenkins /bin/sh
# 检查Jenkins容器内docker环境
docker info
```

查看到相应的输出则正常启动成功。

❗️xiaolin-jenkins容器内的docker环境与宿主机是相通的，共享同一个docker环境。因此在xiaolin-jenkins容器内创建的容器，在宿主机上也能查看到。

## Jenkins快速体验

Web体验，通过浏览器进入宿主机8080端口

- 云服务： ${IP}:8080
- 本地： http://localhost:8080

获取登录密钥，查看日志信息，获取一串密钥，用于Web端登录

```bash
docker logs xiaolin-jenkins
```

Jenkins以插件的方式支持功能扩展，目前已经有1000+插件，除了安装社区推荐的插件外，建议安装以下插件：

- Blue Ocean: BlueOcean Aggregator
- Docker Commons: Provides the common shared functionality for various Docker-related plugins
- Docker Compose Build Step: Docker Compose plugin for Jenkins

## References

[1]. Jenkins用户手册，https://www.jenkins.io/zh/doc/

[2]. Blue Ocean UI，https://www.jenkins.io/zh/doc/book/blueocean/

[3]. Docker，https://www.docker.com/

## 联系作者

1. 在issues中提问
2. 联系邮箱 :email: xing.xiaolin@foxmail.com

<!-- links -->

[contributors-shield]: https://img.shields.io/github/contributors/xiaolinstar/docker-jenkins.svg?style=flat-square
[contributors-url]: https://github.com/xiaolinstar/docker-jenkins/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/xiaolinstar/docker-jenkins.svg?style=flat-square
[forks-url]: https://github.com/xiaolinstar/docker-jenkins/network/members
[stars-shield]: https://img.shields.io/github/stars/xiaolinstar/docker-jenkins.svg?style=flat-square
[stars-url]: https://github.com/xiaolinstar/docker-jenkins/stargazers
[issues-shield]: https://img.shields.io/github/issues/xiaolinstar/docker-jenkins.svg?style=flat-square
[issues-url]: https://github.com/xiaolinstar/docker-jenkins/issues
