# 加载本地内容
## Docker compose
Use .yml file to save the configures for several containers
```
identidock: _1_
build: . _2_
ports: _3_
    - "5000:5000"
environment: _4_
    ENV: DEV
volumes: _5_
    - ./app:/app
```
1. Defines the name of the container
2. Here, we use _build_, it tells we are using the Dockerfile at current path to build container
3. Defines the open port
4. Defines the environment variable
5. Mount local folder './app' as '/app' in the containers


## docker挂载本地目录
docker可以支持把一个宿主机上的目录挂载到镜像里。

交互模式运行
```
docker run -it -v /home/dock/Downloads:/usr/Downloads ubuntu64 /bin/bash
```
后台运行
```
docker run -d -v /home/dock/Downloads:/usr/Downloads --name ubuntu1 ubuntu64
```
通过-v参数，冒号前为宿主机(local)目录，必须为绝对路径，冒号后为镜像内(container)挂载的路径。
现在镜像内就可以共享宿主机里的文件了。

默认挂载的路径权限为读写。如果指定为只读可以用：ro
```
docker run -it -v /home/dock/Downloads:/usr/Downloads:ro ubuntu64 /bin/bash
```
## docker数据卷容器
docker还提供了一种高级的用法。叫数据卷。

数据卷：“其实就是一个正常的容器，专门用来提供数据卷供其它容器挂载的”。感觉像是由一个容器定义的一个
数据挂载信息。其他的容器启动可以直接挂载数据卷容器中定义的挂载信息。

示例：
```
docker run -v /home/dock/Downloads:/usr/Downloads --name dataVol ubuntu64 /bin/bash
```
创建一个普通的容器。用--name给他指定了一个名（不指定的话会生成一个随机的名子）。

再创建一个新的容器，来使用这个数据卷。
```
docker run -it --volumes-from dataVol ubuntu64 /bin/bash
```
--volumes-from用来指定要从哪个数据卷来挂载数据。
这样在新创建的容器里/usr/Downloads目录会和宿主机目录/home/dock/Downloads同步
