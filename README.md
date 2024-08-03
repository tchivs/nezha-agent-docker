# 容器化的 nezha-agent

![x86_64](https://img.shields.io/badge/x86__64-blue) ![amd64](https://img.shields.io/badge/amd64-blue) ![arm64](https://img.shields.io/badge/arm64-blue) ![i386](https://img.shields.io/badge/i386-blue)

[![GitHub stars](https://img.shields.io/github/stars/tchivs/nezha-agent-docker?style=social)](https://github.com/tchivs/nezha-agent-docker) 
[![Docker Pulls](https://img.shields.io/docker/pulls/tchivs/nezha-agent?style=social)](https://hub.docker.com/repository/docker/tchivs/nezha-agent) 

本项目支持通过GitHub Workflow监控nezha-agent的变化，并自动同步打包镜像并推送到Docker Hub。每次代码变更后，GitHub Actions会自动触发构建过程，将最新的镜像推送到Docker Hub仓库。

由于Docker的特性，**宿主机的系统版本（可以自定义）不能被正确读取，网页终端和定时任务功能对宿主机无效。**

---
## 使用教程

### 部署

#### 必须添加的环境变量

| 变量名  | 值       |
| ------- | -------- |
| domain  | 面板域名 |
| secret  | 节点密钥 |

#### 可选添加的环境变量（不添加则使用默认值）

| 变量名  | 值                         | 默认值                |
| ------- | -------------------------- | --------------------- |
| port    | 面板端口                   | 5555                  |
| args    | nezha-agent 支持的额外参数 | --disable-auto-update |
| platform| 系统名                     | 空                    |
| version | 系统版本                   | 空                    |

### Docker Run

```shell
docker run -d -e domain='<面板域名>' -e port='<面板端口>' -e secret='<节点密钥>' -e args='<nezha-agent运行额外参数>' -e platform='<自定义系统名>' -e version='<自定义系统版本>' --net='host' --name='<容器名>' redamancy2319/nezha-agent:latest
```

要多次运行，可重复执行上述命令，并替换为不同面板的参数，但容器名不可重复。

### Docker-Compose

如果你更喜欢使用Docker-Compose进行部署，可以创建一个 `docker-compose.yml` 文件，内容如下：

```yaml
version: '3'
services:
  nezha-agent:
    image: redamancy2319/nezha-agent:latest
    container_name: nezha-agent
    network_mode: host
    environment:
      - domain=<面板域名>
      - port=<面板端口>
      - secret=<节点密钥>
      - args=<nezha-agent运行额外参数>
      - platform=<自定义系统名>
      - version=<自定义系统版本>
```

然后运行以下命令启动容器：

```shell
docker-compose up -d
```

### 关于自定义系统名称和系统版本功能的问题

如果这两个变量中的任何一个被启用，容器内部系统的版本信息将会被修改。这会导致一些软件无法获取正确的版本信息。

**如果需要使用Web Terminal功能，请勿添加platform和version环境变量。**

### 关于Web Terminal功能的使用

Docker的基础镜像为Debian11，为了精简系统占用，一些常用命令可能没有安装。在初次进入终端后需要执行：

```shell
apt-get update
```

这样可以解决 `Unable to locate package` 的问题。

---

## nezha-agent 支持的额外参数

| 参数                       | 说明                     |
| -------------------------- | ------------------------ |
| -d, --debug                | 开启调试信息             |
| --disable-auto-update      | 禁用自动升级             |
| --disable-command-execute  | 禁止在此机器上执行命令   |
| --disable-force-update     | 禁用强制升级             |
| --report-delay int         | 系统状态上报间隔（默认1）|
| --skip-conn                | 不监控连接数             |
| --skip-procs               | 不监控进程数             |
| --tls                      | 启用SSL/TLS加密          |

---

## nezha-agent 支持的系统名

只有特定的系统名会在面板前端（部分主题）显示图标。

| almalinux | alpine     | aosc       | apple       |
| :-------: | :--------: | :--------: | :---------: |
| archlinux | archlabs   | artix      | budgie      |
| centos    | coreos     | debian     | deepin      |
| devuan    | docker     | elementary | fedora      |
| ferris    | flathub    | freebsd    | gentoo      |
| gnu-guix  | illumos    | kali-linux | kali-linux  |
| mageia    | mandriva   | manjaro    | nixos       |
| openbsd   | opensuse   | pop-os     | raspberry-pi|
| redhat    | rocky-linux| sabayon    | slackware   |
| snappy    | solus      | tux        | ubuntu      |
| void      | zorin      |            |             |

---

## 哪吒监控源项目

[官方使用指南](https://nezha.wiki/) |
[哪吒监控GitHub主页](https://github.com/naiba/nezha)
