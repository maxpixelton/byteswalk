# Docker 快速体验

## Docker 是什么

Docker 是一种开源的容器化平台，可以将应用程序及其所有依赖项打包到一个可移植的容器中，以便在任何地方运行。Docker 容器是轻量级、可移植的，具有很高的灵活性和可扩展性，可以帮助开发者更快地构建、打包、部署和运行应用程序。<font color=red>事实上，Docker 已成为容器标准。</font>

Docker 技术是基于 Linux 容器技术的进一步发展，通过使用容器技术，Docker 可以为应用程序提供一个独立的运行环境，避免了由于不同的环境和依赖项引起的兼容性问题。

Docker 的优势包括快速构建、部署和迁移应用程序，提高了应用程序的可靠性和稳定性，同时减少了资源的浪费。Docker 还可以与其他开发工具和平台集成，如 Kubernetes、Jenkins 和 AWS 等，为开发人员提供更加便捷的开发和部署体验。

## Docker 的好处

解释为什么要使用 Docker，就等于解释为什么要使用容器。


### 节省资源

容器相对于虚拟机，容器只需要具备 app 运行的配置和 SDK 即可，不需要创建完整的操作系统，使用的资源大大减少。

### 资源隔离

比如一个虚拟机，跑一个 app，如 redis，没什么问题。但是我想跑不同版本的 redis，那么配置是不是可能冲突，环境是不是受到了污染？

而如果换用容器技术，一个容器就是一个小操作系统，一个虚拟机上可以跑多个容器，那么轻易实现了跑不同版本的 redis，而且还资源隔离，删除容器即卸载了对应版本，是不是非常方便。

### 安装时间大大缩短和灵活移植

一个容器运行了 redis，对应的系统配置和 SDK 依赖都是一样的，那么如果我能将这个容器导出成文件格式，然后再另一台虚拟机上导入运行起来，这样是不是就节省了安装时间呢？没错，容器虚拟技术结合 UnionFile System（联合文件系统）技术就是这么做的。

这样带来的效率提升简直无可言喻。开发以容器虚拟技术开发应用后，将其打包导出，直接交给测试，测试在自己的环境上导入并运行，直接就可以完成测试，大大缩短了应用开发周期。而测试人员完成应用测试后，将其出，直接就可以上线工业环境，缩短了业务上线周期。

甚至将文件开源，建立容器镜像库，全世界的人都可以使用，安装应用只需要下载对应镜像即可快速安装，大大减少了学习成本。


## Docker 引擎

### 核心组件

Docker 引擎是一种基于客户端-服务器（ C/S 架构）模型的应用程序。Docker 核心组件架构就长上图这个样子，由如下主要的组件构成：服务端 Docker 进程（Server Docker Daemon）、API 接口（REST API）、Docker 客户端命令行（Client Docker CLI）。Docker 服务端处理 Docker 客户端的请求，并执行相应的操作，并负责管理容器（Containers）、镜像（Images）、网络（Networks）、数据卷（Data Volumes）。


核心组件的作用分别如下：

1. 服务端 Docker 进程（Server Docker Daemon）：负责管理 Docker 容器和镜像；处理客户端的请求，并执行相应的操作。
2. API 接口（REST API）：为 Docker 客户端提供与服务端 Docker daemon 交互的接口。
3. Docker 客户端命令行（Client Docker CLI）：允许用户与服务端 Docker daemon 交互，客户端发送的一系列请求，例如运行容器、构建镜像等操作，将最终由服务端 Docker daemon 来执行。
4. Docker 镜像（Images）：是一个轻量级、可执行的软件包，其中包含了运行应用程序所需的所有代码、库、配置文件等。容器镜像可以通过运行容器来启动，容器会使用镜像创建一个独立的运行环境，包含容器镜像中的所有文件和依赖项。
5. Docker 容器（Containers）：是 Docker 中的运行时实体，包含了应用程序及其依赖关系，可以被启动、停止、重启、删除等操作。
6. Docker 网络（Networks）：允许容器之间进行通信，并提供网络隔离、端口映射等功能。
7. Docker 存储（Data Volumes）：提供容器内部数据的持久化存储功能，包括数据卷、本地文件系统、网络存储等。

可以通过环境实操来看看这些组件。

1. .通过 `docker info` 命令查看客户端与服务端

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
    ````

    可以看到客户端与服务端的各类信息，如版本等，服务端还记录了各种状态检测。

2. 查看服务端 `Docker daemon`。

    服务端 Docker daemon 是一个守护进程，守护进程就被放置在 centOS 系统的 `/etc/systemd/system/multi-user.target.wants/` 目录下：

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

3. 通过 `docker ps` 命令来查看容器。

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

7. 组件数据的存储位置，进入 `/var/lib/docker` 目录，相关数据都是存储在这里。

    ```shell
    [root@node1 docker]# ll
    total 16
    drwx------.  2 root root   24 Feb  6 21:16 builder
    drwx------.  4 root root   92 Feb  6 21:16 buildkit
    drwx------. 13 root root 4096 Feb  7 02:22 containers
    drwx------.  3 root root   22 Feb  6 21:16 image
    drwxr-x---.  3 root root   19 Feb  6 21:16 network
    drwx------. 57 root root 8192 Feb  7 02:22 overlay2
    drwx------.  4 root root   32 Feb  6 21:16 plugins
    drwx------.  2 root root    6 Feb  7 02:17 runtimes
    drwx------.  2 root root    6 Feb  6 21:16 swarm
    drwx------.  2 root root    6 Feb  7 02:17 tmp
    drwx------.  2 root root    6 Feb  6 21:16 trust
    drwx------.  2 root root   25 Feb  6 21:16 volumes
    ```



## 常用命令

### 容器类

1. `docker run`

2. `docker ps`

3. `docker start、docker restart、docker stop、docker rm、docker kill`

4. `docker exec`

### 镜像类

1. `docker images`

2. `docker rmi`

3. `docker pull`

4. `docker push`

5. `docker tag`

6. `docker commit`

### 查看类

1. `docker inspect`

2. `docker logs`

### 生成类

1. `docker save` 与 `docker load`

2. `docker build`


## 声明周期

Docker 生命周期指的是在 Docker 中创建、运行、停止和删除容器时发生的一系列事件和过程。如下图：


以下是 Docker 生命周期的详细说明：

1. 图中 Dockerfile 是一个重要模块，从编写 Dockerfile 开始 Docker 的第一步，然后通过 docker build 生成镜像（Images）。

2. Docker registry 是我们的镜像仓库，用于保存镜像（docker push）和提供镜像给容器使用（docker pull）。

3. backup.tar 是将我们的镜像导出为一个文件（docker save），这个文件我们可以保存到本地，甚至上传到开源网站供人使用。别人使用时只需要docker load 即可。

4. Images 就是我们本地的镜像，可以自己修改版本号（docker tag），也可以从容器中生成镜像（docker commit）。而镜像就是提供给容器使用的，这是创建容器必不可少的组件。

5. Containers 利用镜像创建并运行一个容器，容器本身可以运行、停止和删除（docker start、docker restart、docker stop、docker rm、docker kill）。

6. 由此我们看出了 Docker 整个生命周期，各种命令的相互之间关系。