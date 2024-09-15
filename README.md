# Jenkins with 「docker in docker」持续集成

> 基于docker-compose快速构建Jenkins容器，且Jenkins容器与宿主机docker环境连通，实现「docker in docker」。

## 前置要求

1. 默认基础环境：amd64 Linux，推荐使用Ubuntu22.04
2. 安装docker和docker-compose

## 快速开始

### 克隆项目

```bash
git clone https://github.com/xiaolinstar/docker-jenkins.git 
```

### 进入项目

```bash
cd docker-jenkins
```

### 检查挂载卷

本项目中`docker-compose.yaml`中的挂载卷值默认为：

```yaml
volumes:
  - '/usr/bin/docker:/usr/bin/docker'
  - '/var/run/docker.sock:/var/run/docker.sock'
  - './jenkins_home:/var/jenkins_home'
```

👀在Windows或macOS中下载Docker Desktop可能存在参数不一致，请自行检查并酌情修改

```yaml
# Macbook Pro M1pro 
# Docker Desktop
volumes:
  - '/usr/local/bin/docker:/usr/bin/docker'
  - '~/.docker/run/docker.sock:/var/run/docker.sock'
  - './jenkins_home:/var/jenkins_home'
```

### 启动容器

```bash
# 创建jenkins_home并在后台启动docker容器
mkdir jenkins_home && docker compose up -d 
```

### 检查容器状态

启动的Jenkins容器名默认为`xiaolin-jenkins`

```bash
docker ps
```

进入`xiaolin-jenkins`容器内部，查看`docker`命令

```bash
# 宿主机执行
docker exec -it xiaolin-jenkins /bin/sh
# 容器内执行
which docker
```

查看到相应的输出则正常启动成功。

❗️xiaolin-jenkins容器内的docker环境与宿主机是相通的，共享同一个docker环境。

## Jenkins快速体验

### Web体验

通过浏览器进入宿主机8080端口

- ${IP}:8080或
- http://localhost:8080

### 获取登录密钥

```bash
docker logs xiaolin-jenkins
```

查看日志信息，获取一串密钥，用于登录Jenkins Web服务
