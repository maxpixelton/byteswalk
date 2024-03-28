# Docker 快速体验

## Docker 是什么

Docker 是一种开源的容器化平台，可以将应用程序及其所有依赖项打包到一个可移植的容器中，以便在任何地方运行，具有轻量、可移植，高灵活性和可扩展性的特点，可以帮助我们更快地 **构建**、**打包**、**部署** 和 **运行** 应用程序。<font color=red>（事实上，Docker 已成为容器标准）</font> 。

Docker 的技术是基于 Linux 容器技术的进一步发展，通过使用容器技术，Docker 可以为应用程序提供一个独立的运行环境，避免了由于不同的环境和依赖项引起的兼容性问题。

Docker 的优势包括快速构建、部署和迁移应用程序，提高了应用程序的可靠性和稳定性，同时减少了资源的浪费。Docker 还可以与其他开发工具和平台集成，如 Kubernetes、Jenkins 和 AWS 等，为我们提供更加便捷的开发和部署体验。

## Docker 的好处

### 节省资源

容器相对于虚拟机，容器只需具备应用程序运行的配置和 SDK 即可，不需要创建完整的操作系统，使用的资源大大减少。

### 资源隔离

比如一个虚拟机，跑一个 app，如 redis，没什么问题。但是我想跑不同版本的 redis，那么配置是不是可能冲突，环境是不是受到了污染 ？

而如果换用容器技术，一个容器就是一个小操作系统，一个虚拟机上可以跑多个容器，那么轻易实现了跑不同版本的 redis，而且还资源隔离，删除容器即卸载了对应版本，是不是非常方便。

### 安装时间大大缩短和灵活移植

一个容器运行了 redis，对应的系统配置和 SDK 依赖都是一样的，那么如果我能将这个容器导出成文件格式，然后再另一台虚拟机上导入运行起来，这样是不是就节省了安装时间呢？没错，容器虚拟技术结合 UnionFile System（联合文件系统）技术就是这么做的。

这样带来的效率提升简直无可言喻。开发以容器虚拟技术开发应用后，将其打包导出，直接交给测试，测试在自己的环境上导入并运行，直接就可以完成测试，大大缩短了应用开发周期。而测试人员完成应用测试后，将其出，直接就可以上线工业环境，缩短了业务上线周期。

甚至将文件开源，建立容器镜像库，全世界的人都可以使用，安装应用只需要下载对应镜像即可快速安装，大大减少了学习成本。

## Docker 引擎

### 核心组件

Docker 引擎是一种基于**客户端-服务器（ C/S 架构）模型**的应用程序。Docker 核心组件架构如下图，主要的组件是：**服务端 Docker 进程（Server Docker Daemon）、API 接口（REST API）、Docker 客户端命令行（Client Docker CLI）。Docker 服务端处理 Docker 客户端的请求，并执行相应的操作，并负责管理容器（Containers）、镜像（Images）、网络（Networks）、数据卷（Data Volumes）**。


核心组件的作用分别如下：

1. 服务端 Docker 进程（Server Docker Daemon）：负责管理 Docker 容器和镜像；处理客户端的请求，并执行相应的操作。
2. API 接口（REST API）：为 Docker 客户端提供与服务端 Docker daemon 交互的接口。
3. Docker 客户端命令行（Client Docker CLI）：允许用户与服务端 Docker daemon 交互，客户端发送的一系列请求，例如运行容器、构建镜像等操作，将最终由服务端 Docker daemon 来执行。
4. Docker 镜像（Images）：是一个轻量级、可执行的软件包，其中包含了运行应用程序所需的所有代码、库、配置文件等。容器镜像可以通过运行容器来启动，容器会使用镜像创建一个独立的运行环境，包含容器镜像中的所有文件和依赖项。
5. Docker 容器（Containers）：是 Docker 中的运行时实体，包含了应用程序及其依赖关系，可以被启动、停止、重启、删除等操作。
6. Docker 网络（Networks）：允许容器之间进行通信，并提供网络隔离、端口映射等功能。
7. Docker 存储（Data Volumes）：提供容器内部数据的持久化存储功能，包括数据卷、本地文件系统、网络存储等。

可以通过环境实操来看看这些组件。

1. 通过 `docker info` 命令查看客户端与服务端：

    ```shell
    Containers: 11
    Running: 4
    Paused: 0
    Stopped: 7
    Images: 5
    Server Version: 18.09.9
    Storage Driver: overlay2
    Backing Filesystem: xfs
    Supports d_type: true
    Native Overlay Diff: true
    Logging Driver: json-file
    Cgroup Driver: systemd
    Plugins:
    Volume: local
    Network: bridge host macvlan null overlay
    Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
    Swarm: inactive
    Runtimes: runc
    Default Runtime: runc
    Init Binary: docker-init
    containerd version: ae07eda36dd25f8a1b98dfbf587313b99c0190bb
    runc version: N/A
    init version: fec3683
    Security Options:
    seccomp
    Profile: default
    Kernel Version: 3.10.0-1160.el7.x86_64
    Operating System: CentOS Linux 7 (Core)
    OSType: linux
    Architecture: x86_64
    CPUs: 1
    Total Memory: 972.3MiB
    Name: node1
    ID: X44V:WXP7:MFBY:BQNT:VOOH:AQTZ:G4KX:QR2K:JA47:ZQ2R:WNNG:NX6G
    Docker Root Dir: /var/lib/docker
    Debug Mode (client): false
    Debug Mode (server): false
    Registry: https://index.docker.io/v1/
    Labels:
    Experimental: false
    Insecure Registries:
    127.0.0.0/8
    Registry Mirrors:
    https://4u6ad4xd.mirror.aliyuncs.com/
    Live Restore Enabled: false
    Product License: Community Engine
    ```

    可以看到客户端与服务端的各类信息，如版本等，服务端还记录了各种状态检测。

2. 查看服务端 `Docker daemon`。服务端 Docker daemon 是一个守护进程，守护进程就被放置在 centOS 系统的 `/etc/systemd/system/multi-user.target.wants/` 目录下：

    ```shell
    [root@node1 multi-user.target.wants]# ls
    auditd.service  chronyd.service  crond.service  docker.service  irqbalance.service  kdump.service  kubelet.service  NetworkManager.service  postfix.service  remote-fs.target  rhel-configure.service  rsyslog.service  sshd.service  tuned.service  vmtoolsd.service
    [root@node1 multi-user.target.wants]# ll
    total 0
    lrwxrwxrwx. 1 root root 38 Feb  6 06:28 auditd.service -> /usr/lib/systemd/system/auditd.service
    lrwxrwxrwx. 1 root root 39 Feb  6 06:28 chronyd.service -> /usr/lib/systemd/system/chronyd.service
    lrwxrwxrwx. 1 root root 37 Feb  6 06:28 crond.service -> /usr/lib/systemd/system/crond.service
    lrwxrwxrwx. 1 root root 38 Feb  6 21:16 docker.service -> /usr/lib/systemd/system/docker.service
    lrwxrwxrwx. 1 root root 42 Feb  6 06:28 irqbalance.service -> /usr/lib/systemd/system/irqbalance.service
    lrwxrwxrwx. 1 root root 37 Feb  6 06:28 kdump.service -> /usr/lib/systemd/system/kdump.service
    lrwxrwxrwx. 1 root root 39 Feb  6 21:18 kubelet.service -> /usr/lib/systemd/system/kubelet.service
    lrwxrwxrwx. 1 root root 46 Feb  6 06:28 NetworkManager.service -> /usr/lib/systemd/system/NetworkManager.service
    lrwxrwxrwx. 1 root root 39 Feb  6 06:28 postfix.service -> /usr/lib/systemd/system/postfix.service
    lrwxrwxrwx. 1 root root 40 Feb  6 06:28 remote-fs.target -> /usr/lib/systemd/system/remote-fs.target
    lrwxrwxrwx. 1 root root 46 Feb  6 06:28 rhel-configure.service -> /usr/lib/systemd/system/rhel-configure.service
    lrwxrwxrwx. 1 root root 39 Feb  6 06:28 rsyslog.service -> /usr/lib/systemd/system/rsyslog.service
    lrwxrwxrwx. 1 root root 36 Feb  6 06:28 sshd.service -> /usr/lib/systemd/system/sshd.service
    lrwxrwxrwx. 1 root root 37 Feb  6 06:28 tuned.service -> /usr/lib/systemd/system/tuned.service
    lrwxrwxrwx. 1 root root 40 Feb  6 06:28 vmtoolsd.service -> /usr/lib/systemd/system/vmtoolsd.service
    ```
    可以看到很多重要的进程都被放在该目录下，如 Docker、kubelet、sshd 等。这些进程会在开机时被 systemd 进程给拉起来。

3. 通过 `docker ps` 命令来查看容器：

    ```shell
    [root@node1 multi-user.target.wants]# docker ps
    CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS               NAMES
    8f3712a32b99        e4430bdf613a           "start_runit"            About an hour ago   Up About an hour                        k8s_calico-node_calico-node-qpnh8_kube-system_849859fa-dc18-4690-9846-ee744e1e5984_2
    2896057d87f5        k8s.gcr.io/pause:3.1   "/pause"                 About an hour ago   Up About an hour                        k8s_POD_calico-node-qpnh8_kube-system_849859fa-dc18-4690-9846-ee744e1e5984_2
    337996f2e7c7        00206e1127f2           "/usr/local/bin/kube…"   About an hour ago   Up About an hour                        k8s_kube-proxy_kube-proxy-jfpz8_kube-system_f4342650-c080-4834-be1a-7c9d9dee25f3_2
    249aaa897107        k8s.gcr.io/pause:3.1   "/pause"                 About an hour ago   Up About an hour                        k8s_POD_kube-proxy-jfpz8_kube-system_f4342650-c080-4834-be1a-7c9d9dee25f3_2
    ```

4. 通过 `docker images` 命令查看镜像信息

    ```shell
    [root@node1 multi-user.target.wants]# docker images
    REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
    calico/node                                          v3.13.5             e4430bdf613a        3 years ago         260MB
    calico/pod2daemon-flexvol                            v3.13.5             ccbc2966dea7        3 years ago         22.8MB
    calico/cni                                           v3.13.5             d39d15fc5cbc        3 years ago         118MB
    k8s.gcr.io/kube-proxy                                v1.15.12            00206e1127f2        3 years ago         82.5MB
    registry.aliyuncs.com/google_containers/kube-proxy   v1.15.12            00206e1127f2        3 years ago         82.5MB
    k8s.gcr.io/pause                                     3.1                 da86e6ba6ca1        6 years ago         742kB
    registry.aliyuncs.com/google_containers/pause        3.1                 da86e6ba6ca1        6 years ago         742kB
    ```

5. 通过 `docker network ls` 命令查看网络信息

    ```shell
    [root@node1 multi-user.target.wants]# docker network ls
    NETWORK ID          NAME                DRIVER              SCOPE
    500d51942ac7        bridge              bridge              local
    6d41a0161930        host                host                local
    6e8ce4e2891e        none                null                local
    ```

    查看具体的信息：

    ```shell
    [root@node1 multi-user.target.wants]# docker network inspect 500d51942ac7
    [
        {
            "Name": "bridge",
            "Id": "500d51942ac7f605fe7e3cccb05dedb677b26c17b8c9ad2c96a432b95761d145",
            "Created": "2024-02-07T02:17:31.898421106-05:00",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": null,
                "Config": [
                    {
                        "Subnet": "172.17.0.0/16",
                        "Gateway": "172.17.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {},
            "Options": {
                "com.docker.network.bridge.default_bridge": "true",
                "com.docker.network.bridge.enable_icc": "true",
                "com.docker.network.bridge.enable_ip_masquerade": "true",
                "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
                "com.docker.network.bridge.name": "docker0",
                "com.docker.network.driver.mtu": "1500"
            },
            "Labels": {}
        }
    ]
    ```

6. 通过 `docker volume ls` 命令查看卷信息

    ```shell
    [root@node1 multi-user.target.wants]# docker volume ls
    DRIVER              VOLUME NAME
    ```

    当前没有给容器挂卷。

7. 组件数据的存储位置，进入 `/var/lib/docker` 目录，相关数据都是存储在这里。

    ```shell
    [root@master docker]# cd /var/lib/docker
    [root@master docker]# pwd
    /var/lib/docker
    [root@master docker]# ll
    total 24
    drwx------.   2 root root    24 Feb  6 21:00 builder
    drwx------.   4 root root    92 Feb  6 21:00 buildkit
    drwx------.  41 root root  4096 Feb  7 03:53 containers
    drwx------.   3 root root    22 Feb  6 21:00 image
    drwxr-x---.   3 root root    19 Feb  6 21:00 network
    drwx------. 124 root root 20480 Mar 21 02:57 overlay2
    drwx------.   4 root root    32 Feb  6 21:00 plugins
    drwx------.   2 root root     6 Mar 21 02:57 runtimes
    drwx------.   2 root root     6 Feb  6 21:00 swarm
    drwx------.   2 root root     6 Mar 21 02:57 tmp
    drwx------.   2 root root     6 Feb  6 21:00 trust
    drwx------.   2 root root    25 Feb  6 21:00 volumes
    [root@master docker]# 
    ```

<br/>

## 常用命令

### 容器类

1. `docker run`

    用来从 Docker 镜像创建运行容器，即从指定的镜像启动一个新的容器实例，并在容器内运行一个命令。例如：`docker run image_name`，基本语法如下：

    ```Shell
    docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
    ```

    其中，`OPTIONS` 用于指定一些选项参数，`IMAGE` 是要创建容器的镜像名称，`COMMAND` 是容器启动后要执行的命令，`ARG...` 是命令的参数。

    `docker run` 命令的一些常用选项包括：

    - `-d`：后台运行容器

    - `-it`：以交互方式运行容器

    - `-p`：将容器的端口映射到宿主机的端口

    - `--name`：指定容器的名称

    - `-v`：将宿主机的目录挂载到容器中

    例如，要从镜像 `ubuntu:lastest` 创建启动一个新的容器，并在容器中运行 `bash` 命令，可以使用以下命令：

    ```shell
    docker run -it --name mycontainer ubuntu:latest bash
    ```
    
    这将创建一个名为 mycontainer 的容器，并以交互方式运行一个 bash shell。

    更多示例命令如下：

    ```shell
    // 从镜像运行容器并在后台运行
    docker run -d --name mycontainer nginx:latest

    // 从镜像运行容器并映射端口
    docker run -d -p 8080:80 --name mycontainer nginx:latest

    // 从镜像运行容器并挂载宿主机目录
    docker run -d -v /path/on/host:/path/in/container --name mycontainer nginx:latest
    ```


2. `docker ps`:

    用来列出正在运行的容器信息。基本语法如下：

    ```shell
    docker ps [OPTIONS]
    ```

    其中，OPTIONS 用来指定一些选项参数，用来筛选要列出的容器信息。

    `docker ps` 命令的一些常用选项包括：

    - `-a`：列出所有容器，包括已停止的容器

    - `-q`： 只列出容器的 ID

    - `-f`：根据条件筛选要列出的容器

    ```shell
    // 列出所有正在运行的容器的信息
    docker ps

    // 列出所有的容器，包括已经停止的容器，可以使用 -a 选项
    docker ps -a

    // 列出容器的 ID，可以使用 -q 选项
    docker ps -q

    // 根据条件筛选要列出的容器，可以使用 -f 选项，例如，要列出名称为 mycontainer 的容器信息，命令如下：
    docker ps -f name=mycontainer
    ```

3. `docker start、docker restart、docker stop、docker rm、docker kill`

    这五个命令属于容器操作命令，分别代表**启动**、**重启**、**停止**、**删除**、**杀死**。使用时，命令后面跟上容器 ID，如 `docker stop container_id`。


4. `docker exec`

    用来在运行中的 Docker 容器中执行命令，该命令可以用于在容器中运行一个新的进程，也可以用于执行一个已经存在的进程。

    在某些情况下， docker exec 可以避免使用 docker attach 命令进入容器内部，这样可以在不中断容器额情况下执行命令。

    docker exec 命令的基本语法如下：
    ```shell
    docker exec [OPTIONS] CONTAINER COMMAND [ARG..]
    ```

    其中，`CONTAINER` 是要执行命令的容器的**名称或ID**，`COMMAND` 是要在容器中执行的命令名称，`ARG...` 是命令的参数。

    常用选项包括：

    - `-d`：在后台执行命令

    - `-i`：允许标准输入流

    - `-t`：为命令分配一个伪终端

    - `-u`：指定要执行命令的用户

    - `-e`：设置环境变量

    例如，要为名 mycontainer 的容器中执行命令 `ls -la`，可以使用以下命令：

    ```shell
    docker exec mycontainer ls -la
    ```

    更多命令示例：

    ```shell
    // 在容器内部以交互模式运行命令，并分配一个伪终端，可以使用 -it 选项
    docker exec -it mycontainer bash

    // 如果想在后台执行命令，可以使用 -d 选项
    docker exec -d mycontainer tail -f /var/log/syslog

    // 在容器内部设置环境变量
    docker exec mycontainer sh -c 'echo "exprot MYVAR=myvalue" >> /etc/profile'

    // 以 root 权限进入容器
    docker exec -itu mycontainer bash
    ```

### 镜像类

1. `docker images`

    用来列出当前所有镜像

2. `docker rmi`

    用来删除一个镜像，例如：`dockcer rmi image_name` 和 `docker rmi image_id`。

3. `docker pull`

    用来拉取一个镜像，例如：`docker pull [url]/image_name:tag`。

    ```shell
    docker pull registry.aliyuncs.com/google_containers/metrics-server:v0.5.2
    ```

    在上面示例中：

    - registry.aliyuncs.com/google_containers 是镜像仓库 url

    - metrics-server 是镜像名称

    - v0.5.2 是镜像 tag，即版本号

    如果没有 url，就默认从 Docker Hub（一个开源的镜像仓库）拉取镜像

4. `docker push`

    用来将一个镜像推送到镜像仓库，例如：`docker push [url]/image_name:tag`。

    ```shell
    docker push registry.aliyuncs.com/google_containers/metrics-server:v0.5.2
    ```

5. `docker tag`

    用来给一个本地的 Docker 镜像打标签，也可以将已有的标签修改为新的标签。

    标签由两部分组成：
    
    - repository：Docker 镜像的名称

    - tag：镜像的版本号或标记

    命令的基本语法是：

    ```shell
    docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
    ```

    其中，`SOURCE_IMAGE` 是源镜像的名称或ID, `TARGET_IMAGE` 是目标镜像的名称或 ID，可以使用 `repository:tag` 的格式指定。

    例如：要将一个名为 myimage 的 Docker 镜像打上 v1.0 的标签，并将其重命名为 `myrepo/myimage:v1.0`，命令如下：

    ```shell
    docker tag myimage myrepo/myimage:v1.0
    ```

    这将在本地的 Docker 镜像中为 myimage 镜像打上 v1.0 的标签，并将其重命名为 `myrepo/myimage:v1.0`。

    如果要修改现有镜像的标签，使用以下命令：

    ```shell
    docker tag myrepo/myimage:oldtag myrepo/myimage:newtag
    ```

    这将修改 `myrepo/myimage` 镜像的标签，将其从 oldtag 修改为 newtag。

    需要注意的是，docker tag 命令只能修改标签，不能修改镜像的 ID。如果要修改镜像的 ID，需要使用 docker commit 命令创建一个新的镜像。

6. `docker commit`

    用来将一个容器的文件系统保存为一个新的镜像。

    通过使用此命令，可以将容器中进行的更改保存为新的镜像，从而不需要重新创建和启动容器。

    基本语法：

    ```shell
    docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
    ```

    其中，`[OPTIONS]` 是可选的命令选项，`CONTAINER` 是要保存为新镜像的`容器的 ID 或名称`，`[REPOSITORY[:TAG]]` 是新镜像的名称和标记。

    常用选项包括:

    - `-a`, `--author`: 设置新镜像的作者信息

    - `-c`, `--change`: 用来在新镜像上执行的 Dockerfile 指令

    - `-m`, `--message`: 为新镜像设置一个描述信息

    - `-p`, `--pause`: 提交时暂停容器的运行

    例如, 将一个名为 mycontainer 的容器保存为新的镜像, 并将其命名为 `myrepo/myimage:v1.0`, 命令如下:
    
    ```shell
    docker commit mycontainer myrepo/myimage:v1.0
    ```

    这将在本地的 Docker 镜像中创建一个名为 `myrepo/myimage` 的新镜像, 其标记为 v1.0, 并且其文件系统与 mycontainer 容器完全相同.

    如果要为新镜像添加作者信息和描述信息,可以使用 `-a` 和 `-m` 选项:

    ```shell
    docker commit -a "jiangdouya" -m "This is my new image" mycontainer myrepo/myimage:v1.0
    ```

    这将为新镜像添加做做信息为 "jiangdouya", 描述信息为 "This is my new image".

    需要注意的是, 使用 docker commit 命令创建的镜像通常比使用 Dockerfile 创建的镜像难以管理和维护. 因此, 建议尽可能使用 Dockerfile 来创建和管理 Docker 镜像.

### 查看类

1. `docker inspect`

    用来获取 Docker 对象（容器、镜像、网络）的详细信息。通过使用这个命令，我们可以查看有关容器、镜像、网络等对象的所有属性信息，例如容器的 IP 地址、端口映射、挂载的卷等。基本语法是：`docker inspect [OPTIONS] OBJECT [OBJECT...]`。

    其中，[OPTIONS] 是可选的命令选项，OBJECT 是要检查的 Docker 对象的 ID 或名称，可以同时指定多个 Docker 对象。

    常用选项包括：

    - `-f`、`--format`：设置输出格式

    - `--type`：指定要检查的 Docker 对象类型（container、image、network 等）

    - `--size`：显示与 Docker 对象相关联的数据卷的大小信息

    例如，要获取名为 mycontainer 的容器的详细信息，命令如下：
    
    ```shell
    # 返回一个 JSON 格式的输出，包含有关 mycontainer 容器的所有详细信息
    docker inspect mycontainer
    ```

    如果要只显示容器的 IP 地址, 可以使用 -f 选项设置输出格式, 如下:

    ```shell
    # 这里只显示容器的 IP 地址
    docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer
    ```

    !!! tip

        需要注意的是, `docker inpest` 命令返回的信息非常详细, 但也非常复杂, 如果只需要查看特定属性的值, 建议使用 `grep` 等命令过来输出.


2. `docker logs`

    该命令用来查看容器的日志输出. 当容器运行时, 它会在标准输出和标准错误输出中生产日志消息, 从而了解容器的运行状态和任何错误或异常情况.

    ```shell
    // 查看正在运行的容器的输出
    docker logs <container_id_or_name>

    // 查看容器的最后 10 条日志消息
    docker logs --tail 10 <container_id_or_name>

    // 实时查看容器的日志输出
    docker logs --follow <container_id_or_name>
    ```

    

### 生成类

1. `docker save` 与 `docker load`

    `docker save` 是一个命令行工具，用来将 Docker 镜像文件保存为 .tar 文件。通过该命令，我们可以将镜像保存到本地或远程服务器并与其他人共享。

    `docker save` 命令的语法：`docker save [OPTIONS] IMAGE [IMAGE...]`

    其中，`[OPTIONS]` 表示可选参数，`IMAGE` 表示要保存的镜像名称。

    常用的选项有：
    - `-o`, `--output`：指定输出文件名，默认 `STDOUT`。

    - `-q`, `--quiet`：仅输出 SHA256 哈希值，不显示进度信息。

    - `--tag`：指定要保存的镜像的标签名称。

    比如，将名为 "myimage:latest" 的镜像保存为文件 "myimage.tar"

    ```shell
    docker save -o myimage.tar myimage:latest
    ```

    相反的，使用 `docker load` 命令将 .tar 文件加载回 Docker 镜像库中

    ```shell
    docker load -i myimage.tar
    ```

    再使用 `dokcer tag` 将加载的镜像改为指定的 tag。
   

2. `docker build`

    `docker build` 也是一个命令行工具，根据 Dockerfile 构建一个新的 Docker 镜像。Docker 是一个文本文件，它包含一些列的指令和参数，用来定义如何构建 Docker 镜像。使用 `docker build` 命令可以自动化构建和管理 Docker 镜像的过程：

    `docker build` 命令的语法是：`docker build [OPTIONS] PATH | URL | -`

    其中，`[OPTIONS]` 表示可选参数，`PATH`、`UTL` 或 `-` 表示 Dockerfile 所在的路径，通常情况下，我们可以在 Dockerfile 所在的目录中执行该命令：

    下面是一些常用的选项：

    - `-f`、`-file`：指定 Dockerfile 的文件名，默认为 `Dockerfile`。

    - `-t`、`--tag`：为构建的镜像指定一个名称和标签。

    - `--build-arg`：设置构建参数，格式为 key=value。

    - `--no-cache`：不使用换成，强制重新构建 Docker 镜像。

    例如，假设有一个 Dockerfile 文件，内容如下：

    ```Dockerfile
    FROM ubuntu:lastest
    RUN apt-get update && apt-get install -y nginx
    CMD ["nginx", "-g", "daemon off:"]
    ```

    使用以下命令在该 Dockerfile 所在的目录中构建一个新的 Docker 镜像，并将其命名为 "mynginx:latest"：

    ```java
    docker build -t mynginx:latest
    ```

    此命令将会读取当前目录中的 Dockerfile 文件，然后构建一个新的镜像。一旦构建完成，可以使用 docker run 命令启动这个镜像中的容器：

    ```shell
    docker run -p 80:80 mynginx:latest
    ```

    这将启动一个新的 nginx 容器，可以在本地主机的 80 端口访问它。


## 生命周期

Docker 生命周期指的是在 Docker 中创建、运行、停止和删除容器时发生的一系列事件和过程。如下图：


以下是 Docker 生命周期的详细说明：

1. 图中 Dockerfile 是一个重要模块，从编写 Dockerfile 开始 Docker 的第一步，然后通过 docker build 生成镜像（Images）。

2. Docker registry 是我们的镜像仓库，用于保存镜像（docker push）和提供镜像给容器使用（docker pull）。

3. backup.tar 是将我们的镜像导出为一个文件（docker save），这个文件我们可以保存到本地，甚至上传到开源网站供人使用。别人使用时只需要docker load 即可。

4. Images 就是我们本地的镜像，可以自己修改版本号（docker tag），也可以从容器中生成镜像（docker commit）。而镜像就是提供给容器使用的，这是创建容器必不可少的组件。

5. Containers 利用镜像创建并运行一个容器，容器本身可以运行、停止和删除（docker start、docker restart、docker stop、docker rm、docker kill）。

6. 由此我们看出了 Docker 整个生命周期，各种命令的相互之间关系。