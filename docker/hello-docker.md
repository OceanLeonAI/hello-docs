# 各种常用命令

## Docker

### 镜像仓库

#### search

> docker search [OPTIONS] TERM
>
> OPTIONS说明：
>
> - **--automated :**只列出 automated build类型的镜像；
> - **--no-trunc :**显示完整的镜像描述；
> - **-f <过滤条件>:**列出收藏数不小于指定值的镜像。

1.  从 Docker Hub 查找所有镜像名包含 java，并且收藏数大于 10 的镜像

> docker search -f stars=10 java

NAME                               DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
node                               Node.js is a JavaScript-based platform for s…   10924     [OK]
tomcat                             Apache Tomcat is an open source implementati…   3205      [OK]
openjdk                            OpenJDK is an open-source implementation of …   3080      [OK]
java                               DEPRECATED; use "openjdk" (or other JDK impl…   1976      [OK]
ghost                              Ghost is a free and open source blogging pla…   1460      [OK]
couchdb                            CouchDB is a database that uses JSON for doc…   452       [OK]
jetty                              Jetty provides a Web server and javax.servle…   379       [OK]
amazoncorretto                     Corretto is a no-cost, production-ready dist…   180       [OK]
groovy                             Apache Groovy is a multi-faceted language fo…   121       [OK]
lwieske/java-8                     Oracle Java 8 Container - Full + Slim - Base…   50                   [OK]
nimmis/java-centos                 This is docker images of CentOS 7 with diffe…   42                   [OK]
fabric8/java-jboss-openjdk8-jdk    Fabric8 Java Base Image (JBoss, OpenJDK 8)      29                   [OK]
timbru31/java-node                 OpenJDK JRE or JDK (8 or 11) with Node.js 12…   19                   [OK]
cloudbees/java-build-tools         Docker image with commonly used tools to bui…   16                   [OK]
fabric8/java-alpine-openjdk8-jre   Fabric8 Java Base Image (Alpine, OpenJDK 8, …   13                   [OK]
frekele/java                       docker run --rm --name java frekele/java        12                   [OK]

参数说明：

**NAME:** 镜像仓库源的名称

**DESCRIPTION:** 镜像的描述

**OFFICIAL:** 是否 docker 官方发布

**stars:** 类似 Github 里面的 star，表示点赞、喜欢的意思。

**AUTOMATED:** 自动构建。

#### pull

> docker pull [OPTIONS] NAME[:TAG|@DIGEST]
>
> OPTIONS说明：
>
> - **-a :**拉取所有 tagged 镜像
>
>
>
> - **--disable-content-trust :**忽略镜像的校验,默认开启

1.  从Docker Hub下载java最新版镜像。

> docker pull java

2. 从Docker Hub下载REPOSITORY为java的所有镜像。

   > docker pull -a java

### 本地镜像管理

#### images

> docker images [OPTIONS] [REPOSITORY[:TAG]]
>
> OPTIONS说明：
>
> - **-a :**列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
>
>
>
> - **--digests :**显示镜像的摘要信息；
>
>
>
> - **-f :**显示满足条件的镜像；
>
>
>
> - **--format :**指定返回值的模板文件；
>
>
>
> - **--no-trunc :**显示完整的镜像信息；
>
>
>
> - **-q :**只显示镜像ID。

1.  查看本地镜像列表

> docker images

2.  列出本地镜像中REPOSITORY为hello-world的镜像列表

> docker images hello-world

#### rmi

> docker rm [OPTIONS] CONTAINER [CONTAINER...]
>
> OPTIONS说明：
>
> - **-f :**强制删除；
>
>
>
> - **--no-prune :**不移除该镜像的过程镜像，默认移除；

1.  强制删除本地镜像 hello-world/hello-world:latest。

> docker rmi -f hello-world/hello-world:latest

### 容器生命周期管理

#### run

> ocker run [OPTIONS] IMAGE [COMMAND] [ARG...]
>
> OPTIONS说明：
>
> - **-a stdin:** 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
> - **-d:** 后台运行容器，并返回容器ID；
> - **-i:** 以交互模式运行容器，通常与 -t 同时使用；
> - **-P:** 随机端口映射，容器内部端口**随机**映射到主机的端口
> - **-p:** 指定端口映射，格式为：**主机(宿主)端口:容器端口**
> - **-t:** 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
> - **--name="nginx-lb":** 为容器指定一个名称；
> - **--dns 8.8.8.8:** 指定容器使用的DNS服务器，默认和宿主一致；
> - **--dns-search example.com:** 指定容器DNS搜索域名，默认和宿主一致；
> - **-h "mars":** 指定容器的hostname；
> - **-e username="ritchie":** 设置环境变量；
> - **--env-file=[]:** 从指定文件读入环境变量；
> - **--cpuset="0-2" or --cpuset="0,1,2":** 绑定容器到指定CPU运行；
> - **-m :**设置容器使用内存最大值；
> - **--net="bridge":** 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
> - **--link=[]:** 添加链接到另一个容器；
> - **--expose=[]:** 开放一个端口或一组端口；
> - **--volume , -v:** 绑定一个卷

1.  使用docker镜像nginx:latest以后台模式启动一个容器,并将容器命名为mynginx。

> docker run --name mynginx -d nginx:latest

2.  使用镜像nginx:latest以后台模式启动一个容器,并将容器的80端口映射到主机随机端口。

> docker run -P -d nginx:latest

3.  使用镜像 nginx:latest，以后台模式启动一个容器,将容器的 80 端口映射到主机的 80 端口,主机的目录 /data 映射到容器的 /data。

> docker run -p 80:80 -v /data:/data -d nginx:latest

4.  绑定容器的 8080 端口，并将其映射到本地主机 127.0.0.1 的 80 端口上。

> docker run -p 127.0.0.1:80:8080/tcp ubuntu bash

5.  使用镜像nginx:latest以交互模式启动一个容器,在容器内执行/bin/bash命令。

> docker run -it nginx:latest /bin/bash

6. 启动 redis

   > docker run -d --name redis -p 6379:6379 redis:latest

#### exec

> docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
>
> OPTIONS说明：
>
> - **-d :**分离模式: 在后台运行
> - **-i :**即使没有附加也保持STDIN 打开
> - **-t :**分配一个伪终端

1.  在容器 mynginx 中以交互模式执行容器内 /root/runoob.sh 脚本:

> docker exec -it mynginx /bin/sh /root/runoob.sh

2.  在容器 mynginx 中开启一个交互模式的终端:

> docker exec -i -t  mynginx /bin/bash

### 

