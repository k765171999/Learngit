# Learn Docker

## <a href = "https://vuepress.mirror.docker-practice.com/image/pull.html">参考资料</a>

## 使用镜像

### 获取

`docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]`

- Docker 镜像仓库地址：地址的格式一般是 `<域名/IP>[:端口号]`。默认地址是 Docker Hub。
- 仓库名：如之前所说，这里的仓库名是两段式名称，即 `<用户名>/<软件名>`。对于 Docker Hub，如果不给出用户名，则默认为 `library`，也就是官方镜像。

- `docker pull ubuntu:18.04`这其中省略了镜像仓库地址，和仓库名字，所以就是默认的使用的镜像仓库地址是DockerHub，仓库名是library，具体就是library/ubuntu

### 运行镜像

```
docker run -it --rm `
ubuntu:latest `
bash

cat /etc/os-release

exit
```

- 首先在这个代码中的`-it`是两个的结合，分别是`-i`和`-t`，i代表了交互，t代表了终端，就是交互终端。
- `--rm`是指在退出之后，删除。
- bash是交互命令

> 与Linux系统不一样，在windows powerShell中的多行命令使用的是 \`，Linux使用\

- `cat /etc/os-release`这是个bash命令，用来查看系统的信息用的
- 最后使用完用exit退出系统

### 列出镜像

1. 查信息

```
docker image ls  
// 这个可以查看所有已经下载下载的镜像。
// 其中的<none>代表已经更新，没用了

docker system df
// 查看镜像使用的体积

docker image ls -f dangling=true
// -f是filter的意思，过滤器
// <none>是悬挂镜像，一般悬挂镜像是没有价值的了

docker image prune 
// 清楚没有用的悬挂镜像


docker image ls -a
// 列出所有镜像，包括中间层

docker image ls ubuntu:18.04
docker image ls -f label=com.example.version=0.1
// 一些利用-f的例子

docker image ls -q
// 特定格式显示

docker image ls --format "{{.ID}}: {{.Repository}}"
docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
// 用--format控制格式


```

2. 删除镜像

```
docker image rm ID or 长编号 or 短编号 or 镜像摘要
// 分为untagged 和 deleted两个步骤，有些还有用的悬挂镜像（中间层）会untagged但是不会delete，当所有的tag都被取消了，那么久要删除了


docker image rm $(docker image ls -q redis)
docker image rm $(docker image ls -q -f before=mongo:3.2)
//可以使用这种指令集合，来删除
```

## 操作容器

### 启动容器

```
docker run ubuntu:latest /bin/echo "hello world"
// 新建并启动一个容器

docker run -t -i ubuntu:18.04 /bin/bash
// 开启bash交互终端
```

过程：

- 检查本地是否存在指定的镜像，不存在就从公有仓库下载
- 利用镜像创建并启动一个容器
- 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
- 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
- 从地址池配置一个 ip 地址给容器
- 执行用户指定的应用程序
- 执行完毕后容器被终止

```
docker container start 容器名字
// 重新启动已经关闭的容器

pwd 路径
ps 进程
```

### 后台运行

一般使用docker，会选择后台让他运行，可以用`-d`来实现

```
docker run ubuntu:latest /bin/sh -c "while true; do echo hello world; sleep 1; done"
// 使用这个命令，会在控制台上一直打印helloworld，但是中断后，就停了，没有后台

docker run -d ubuntu:latest /bin/sh -c "while true; do echo hello world; sleep 1; done"
//添加上 -d 就可以持续后台运行

docker logs containername 
// 使用logs 可以查看打印结果

docker container ls
// 类似于 docker image ls
// 显示运行中container的名字

docker container logs [container name or ID]
// 可以查看容器输出信息
```

### 终止容器

````
docker container stop [container name or ID]
// 停止某个容器
````

### 进入容器

```
docker attach
docker exec

docker attach test2
// 使用这个命令可以进入到终端之中，但是最后输入exit，这个容器会直接关闭了

docker exec -it test2 bash
// exec可以支持更多的命令，并且在进去之后输入exit，不会关闭容器
// docker exec --help
```

### 删除容器

```
docker container rm containername
// 删除容器

docker container prune
// 删除所有处于终止状态的容器
```

### 导入导出容器

```

```

## 数据管理

### 数据卷

```
docker volume create my-vol
// 创建一个数据卷

docker volume ls
// 查看所有数据卷

docker volume inspect my-vol
// 查看数据卷信息

docker run -d -P \
    --name web \
    --mount source=my-vol,target=/webapp \
    training/webapp \
    python app.py
// 启动一个挂载数据卷的容器到webapp

docker inspect web 
// 查看Mounts挂载数据卷信息

docker volume rm my-vol
// 删除数据卷

docker volume prune
// 删除所有无主数据卷
```

### 挂载主机目录

```
docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp \
    --mount type=bind,source=/src/webapp,target=/opt/webapp \
    training/webapp \
    python app.py
// 挂载一个主机目录到容器中
// 使用--mount时，如果主机没有source这个目录会报错，而-v会直接创建

docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp:ro \
    --mount type=bind,source=/src/webapp,target=/opt/webapp,readonly \
    training/webapp \
    python app.py
// 设置为只读

docker run --rm -it \
   # -v $HOME/.bash_history:/root/.bash_history \
   --mount type=bind,source=$HOME/.bash_history,target=/root/.bash_history \
   ubuntu:18.04 \
   bash
// 挂载本地主机目录
```

## 使用网络

