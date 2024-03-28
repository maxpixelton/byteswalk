# 安装 Docker + K8s


## 安装 Dokcer

1. 安装依赖：`yum install -y yum-utils device-mapper-persistent-data lvm2 wget`

2. 设置阿里镜像仓库（Docker 默认是国外，建议改为国内的阿里镜像仓库，提供运行速度）：`yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`

3. 更新 yum 索引：`yum makecache fast`

4. 安装 docker-ce 社区版：`yum -y install docker-ce docker-ce-cli containerd.io`

5. 配置阿里云镜像加速器

    !!! tips

        ![阿里云镜像加速器](https://shichuan-hao.github.io/images/static/cloud/quick-install-docker-k8s-imagejiasu.png){width=290 higth=400 align=right loading=lazy}

        1. 阿里云镜像加速器是免费的。

        2. 登录账号在下面找到配置信息：

            ```shell
            sudo mkdir -p /etc/docker
            sudo tee /etc/docker/daemon.json <<-'EOF'
            {
            "registry-mirrors": ["https://ua8gllj7.mirror.aliyuncs.com"]
            }
            EOF
            sudo systemctl daemon-reload
            sudo systemctl restart docker
            ```
          
        3. 检查配置。执行命令 `docker info`，如果看到 `docker info | grep "Registry Mirrors" -A3`，说明配置成功。


## 安装 etcd

Etcd 是一个开源的分布式键值存储系统，由 CoreOS 开发并维护，是 Kubernetes 集群的核心组件之一。

Etcd 被设计用于存储 Kubernetes 集群中的所有关键数据，如集群状态、Pod 和 Service 的配置信息、网络拓扑等等。

在 Kubernetes 中， etcd 是控制平面的一部分，负责存储和同步集群的元数据信息。

下面两种安装方式选择任意一个即可：

1. `yum -y install etcd`。

2. 使用二进制安装也是可以的，参见：[🚀传送门](https://blog.csdn.net/Mr_XiMu/article/details/125026635)。

## 安装 kubelet、kubeadm、kubectl

- __Kubelet__，k8s 集群中的一个核心组件：

    * 每个节点上的代理。
    * 负责维护节点上容器的生命周期，根据集群的配置和调度决策，启动、停止、重启和销毁容器，并监控它们的状态，
    * 管理容器和节点之间的网络，确保容器能够相互通信，同时也支持网络插件的扩展。

- __Kubeadm__，快速安装 k8s 的工具：

    * 通过它我们可以轻松地初始化一个 k8s 控制平面和节点，创建和安装必要的组件和服务，如 kubelet 和 kube-proxy。
    * 其设计目标就是使 k8s 集群的初始化和管理变得更加简单和可靠。

- __kubectl__，k8s 的命令行界面工具：

    * 管理和操作 k8s 集群的主要方式之一。
    * 可以通过简单的命令行指令来管理 k8s 集群中的各种资源对象，如 Pod、Service、Deployment、ConfigMap 等等。

安装过程如下：

1. 配置 yum 源：

    ```shell
    cat <<EOF > /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes Repository
    name=Kubernetes
    baseurl=http://mirrors.ustc.edu.cn/kubernetes/yum/repos/kubernetes-el7-x86_64/
    enabled=1
    gpgcheck=0
    repo_gpgcheck=0
    gpgkey=https://mirrors.ustc.edu.cn/kubernetes/yum/doc/yum-key.gpg https://mirrors.ustc.edu.cn/kubernetes/yum/doc/rpm-package-key.gpg
    exclude=kubelet kubeadm kubectl
    EOF
    ```

2. 更新 yum 索引：`yum makecache fast`。

3. 列出所有可安装的版本：`yum --showduplicates list kubeadm` 。

4. 安装指定版本：`yum -y install kubeadm-1.15.5 kubectl-1.15.5 kubelet-1.15.5 --disableexcludes=kubernetes` 。

5. 前置操作

    1. 关闭 swap：`swapoff -a`。

    2. 关闭防火墙：
        ```shell
        ## 暂时关闭防火墙
        systemctl stop firewalld.service
        ## 永久关闭
        systemctl disable firewalld.service
        ```
    
    3. 指定使用 systemed 作为本机（native）控制组（cgroup）驱动程序：
        ```shell
        vi /etc/docker/daemon.json
        {
        "exec-opts": ["native.cgroupdriver=systemd"]
        }
        sudo systemctl daemon-reload
        sudo systemctl restart docker
        ```

        ![添加 exec-opts](https://shichuan-hao.github.io/images/static/cloud/add-exec-opts-docker.png)

6. 设置 kubelet 开机启动：`systemctl daemon-reload && systemctl enable kubelet && systemctl start kubelet`。

7. 生成 master 节点配置 kubeadm 的初始化文件：`kubeadm config print init-defaults > init.default.yaml`，需要修改的配置如下：

    ![master 节点配置 kubeadm 的初始化文件](https://shichuan-hao.github.io/images/static/cloud/inital-config-file-with-master-node.png)

    本实验修改如下：
    ```yaml
    advertiseAddress: 192.168.80.30
    criSocket: /var/run/dockershim.sock （注意，如果runc是containerd，这里需要修改为/run/containerd/containerd.sock）
    imageRepository: registry.aliyuncs.com/google_containers
    kubernetesVersion: v1.15.5
    ```

8. 提前下载需要的镜像

    !!! tips "容器镜像 ？"

        - 一种轻量级、可移植的软件打包方式，可以将应用程序及其依赖项打包在一起，以便在不同的计算机上运行。
        
        - 通常包含应用程序、库、配置文件和其他依赖项，以及操作系统的部分或全部文件系统。
        
        - 采用层次结构存储，每一层都包含一个文件系统的快照，可以在容器中通过层叠起来形成一个完整的文件系统。

        - 通过运行容器来启动，容器会使用镜像创建一个独立的运行环境，包含容器镜像中的所有文件和依赖项。

        通俗来说，容器镜像为容器运行提供所需的运行环境。一个容器的创建运行依赖于镜像。所以为了运行 kubelet、kubectl 等服务，我们需要将镜像提前下载好。

    1. 查看需要的镜像

        ```shell
        [root@master ~]# kubeadm config images list
        I0206 21:07:05.491815    2505 version.go:248] remote version is much newer: v1.29.1; falling back to: stable-1.15
        k8s.gcr.io/kube-apiserver:v1.15.12
        k8s.gcr.io/kube-controller-manager:v1.15.12
        k8s.gcr.io/kube-scheduler:v1.15.12
        k8s.gcr.io/kube-proxy:v1.15.12
        k8s.gcr.io/pause:3.1
        k8s.gcr.io/etcd:3.3.10
        k8s.gcr.io/coredns:1.3.1
        ```

    2. 提前下载好需要的镜像，命令是：`kubeadm config images pull --image-repository registry.aliyuncs.com/google_containers`

        <font color=red>此步骤非常重要，因为总是会因为网络问题导致镜像下载不下来，进而导致安装 kubeadm 失败</font>

        ![下载好的镜像](https://shichuan-hao.github.io/images/static/cloud/kudeadm-images-pull.png)

    3. 修改镜像标签 tag。下载好的镜像地址名称和要求的不一样，因此需要修改镜像 tag 保持一致：`dokcker tag <源镜像> <目标镜像>`

        ```shell
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/kube-apiserver:v1.15.12 k8s.gcr.io/kube-apiserver:v1.15.12
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/kube-controller-manager:v1.15.12 k8s.gcr.io/kube-controller-manager:v1.15.12
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/kube-proxy:v1.15.12 k8s.gcr.io/kube-proxy:v1.15.12
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/kube-scheduler:v1.15.12 k8s.gcr.io/kube-scheduler:v1.15.12
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/coredns:1.3.1 k8s.gcr.io/coredns:1.3.1
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/etcd:3.3.10 k8s.gcr.io/etcd:3.3.10
        [root@master ~]# docker tag registry.aliyuncs.com/google_containers/pause:3.1 k8s.gcr.io/pause:3.1
        ```

        如果有个别镜像重试几次都无法下载，就单独处理：`docker pull <镜像名>`，不停换仓库想办法把镜像下载下来，然后修改 tag

9. 安装前检查：`kubeadm init phase preflight`，<font color=red>如果检查出来报错，一定要先处理！</font>

10. 初始化 kubeadm：`kubeadm init --ignore-preflight-errors=ImagePull --pod-network-cidr=10.244.0.0/16`。

    <font color=red>10.244.0.0/16 为 k8s 内部的 pod 节点之间网络可以使用的 IP 段，尽量不和 service-cidr 一样。</font>

    ![安装成功提示](https://shichuan-hao.github.io/images/static/cloud/kubernetes-control-plane-initialized-successfully.png)

    安装成功，要将下面的内容记下来（🔥🔥🔥），用来纳管其他节点进入集群时需要：

    ```shell
    Then you can join any number of worker nodes by running the following on each as root:

    kubeadm join 192.168.80.130:6443 --token w07wvl.c39j32a4n6rr1qft \
        --discovery-token-ca-cert-hash sha256:b93546cc80fb608115b258eda44754eac90bb6169b660610b4103d890670dc35 
    ```

11. 检查安装结果：`kubectl version`，输入内容如下：

    ```
    [root@master ~]# kubectl version
    Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.5", GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", GitTreeState:"clean", BuildDate:"2019-10-15T19:16:51Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}
    The connection to the server localhost:8080 was refused - did you specify the right host or port?
    ```

<font color=red size=5>安装完成！</font>

<hr/>


## 问题处理


??? Bug "The connection to the server localhost:8080 was refused - did you specify the right host or port?"

    执行 `kubectl get node` 或 `kubectl version` 命令时报错，

    ```shell
    [root@master ~]# kubectl version
    Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.5", GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", GitTreeState:"clean", BuildDate:"2019-10-15T19:16:51Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}
    The connection to the server localhost:8080 was refused - did you specify the right host or port?
    ```

    查看不到结果，需要配置：

    ```shell
    [root@master ~]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /etc/profile
    [root@master ~]# source /etc/profile
    [root@master ~]# echo "export KUBECONFIG=/etc/kubernetes/kubelet.conf" >> /etc/profile
    [root@master ~]# source /etc/profile
    [root@master ~]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /etc/profile
    [root@master ~]# source /etc/profile
    ```

    再次查询

    ```shell
    [root@master ~]# kubectl get node
    NAME     STATUS     ROLES    AGE    VERSION
    master   NotReady   master   108s   v1.15.5
    ```

    <font color=red>完成 ！</font>

    如果依然无法查询，那么看看 docker 状态：`systemctl status docker`。

    docker 是好的，那么看看 kubelet 状态：`systemctl status kubelet`。

    以此往复，查看报错信息，见招拆招。


??? Bug "发现有 Pod 处于 pending 状态 ！"


    通过命令 `kubectl describe pod coredns-5c98db65d4-pbctm -n kube-system | tail -20`，查看输出信息。使用到的命令：

    - 查看 node 状态：kubectl get node

    - 看 kubelet 状态：systemctl status kubelet

    发现节点处于 notready 状态，可能 kubelet 无法上报节点状态，那么查看 kubelet 状态。

    发现 kubelet 启动异常，原因是 cni 有问题，即网络插件有问题。由于没有设置集群网络，安装配置完网络会变为 Ready 状态。


??? Bug "加入k8s集群报错this Docker version is not on the list of validated versions: 20.10.17. Latest validated..."

    详细报错信息：
    ```shell
    [preflight] Running pre-flight checks
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 20.10.17. Latest validated version: 18.09
    ```

    报错信息可以看出跟docker的版本有关系，意思是：此 Docker 版本不在已验证版本列表中：20.10.17。 最新验证版本：18.09

    分别查看 docker和 k8s的版本

    - kubectl version

    - docker version

    通过查看 k8s 与 docker 的兼容关系图，所以需要降低 docker的版本到 18.09 以下：

    - 查看当前仓库支持的 docker 版本：`yum list docker-ce --showduplicates | sort -r`

    - 降低 docker 版本到 18.06.3.ce-3.el7：`yum downgrade --setopt=obsoletes=0 -y docker-ce-18.09.9-3.el7 docker-ce-cli-18.09.9-3.el7 containerd.io`

## 加入数据节点进入集群

1. 创建虚拟机。

2. 做好前置配置，如关闭 swap、防火墙等。

3. 安装软件。步骤和 master 节点的安装一致，安装好 docker、kubeadm、kubelet。注意，这一步不用安装 kubectl。

4. 设置 kubelet 开机启动：`systemctl daemon-reload && systemctl enable kubelet && systemctl start kubelet`

5. 安装 master 时最后的回显信息，就是我们用来纳管节点的命令：

    ```shell
    kubeadm join 192.168.80.130:6443 --token w07wvl.c39j32a4n6rr1qft \
    --discovery-token-ca-cert-hash sha256:b93546cc80fb608115b258eda44754eac90bb6169b660610b4103d890670dc35
    ```

6. 检查。

    ```shell
    [root@master kubernetes]# kubectl get node

    NAME     STATUS     ROLES    AGE   VERSION
    master   NotReady   master   10m   v1.15.5
    node1    NotReady   <none>   11s   v1.15.5
    ```

    这个时候我们的集群已经有了两个节点了。但是节点是 NotReady 状态，因为这时还没有安装网络插件（CNI）。

    网络插件（CNI）是 Kubernetes 集群中的一个重要组件，它负责管理容器和节点之间的网络通信，确保容器能够相互通信，以及与外部网络进行交互。典型的网络插件如 flannel 和 Calico。

## 安装网络插件 calico

1. 将 calico 保存到本地

    选择 calico 的时候，要注意匹配的版本，我这里选择的是和 1.5.5 k8s 对应的 3.13 版本的 calico。所以要根据实际版本下载，修改 v3.13 即可。


    查询网址：https://projectcalico.docs.tigera.io/archive/v3.13/getting-started/kubernetes/requirements

    将 calico 保存到本地

    curl https://docs.projectcalico.org/archive/v3.13/manifests/calico.yaml -O


2.  service 配置文件

    该 yaml 文件中 CIDR 默认是 192.168.0.0/16，需要与初始化时 kube-config.yaml 中的配置一致，我的是 kubeadm 部署的 k8s，使用的是 10.244.0.0/16

    ```yaml
    - name: CALICO_IPV4POOL_CIDR
      value: "10.244.0.0/16"
    ```

    指定网卡，我的网卡是 ens33，这里增加两行。

    ```yaml
    - name: IP_AUTODETECTION_METHOD
      value: "interface=ens33"
    ```

3. 部署：`kubectl apply -f calico.yaml`

4. 状态检查：`kubectl get pod -A`

    ```shell
    [root@master ~]# kubectl get pod -A
    NAMESPACE     NAME                                       READY   STATUS              RESTARTS   AGE
    kube-system   calico-kube-controllers-659f4b66fd-hg8tg   0/1     Pending             0          24s
    kube-system   calico-node-gl8tz                          0/1     Init:0/3            0          23s
    kube-system   calico-node-qpnh8                          0/1     Init:0/3            0          23s
    kube-system   coredns-5c98db65d4-7qrs8                   0/1     Pending             0          18m
    kube-system   coredns-5c98db65d4-mpvpr                   0/1     Pending             0          18m
    kube-system   etcd-master                                1/1     Running             0          17m
    kube-system   kube-apiserver-master                      1/1     Running             0          17m
    kube-system   kube-controller-manager-master             1/1     Running             0          17m
    kube-system   kube-proxy-g6tqp                           1/1     Running             0          18m
    kube-system   kube-proxy-jfpz8                           0/1     ContainerCreating   0          9m21s
    kube-system   kube-scheduler-master                      1/1     Running             0          17m
    ```

    发现有些 Pod 异常，我们 kubectl describe kube-flannel-ds-ch66n 看看它的状态：

    ```shell
    [root@master ~]# kubectl describe pod calico-node-qpnh8 -n kube-system | tail -10
                 node.kubernetes.io/network-unavailable:NoSchedule
                 node.kubernetes.io/not-ready:NoExecute
                 node.kubernetes.io/pid-pressure:NoSchedule
                 node.kubernetes.io/unreachable:NoExecute
                 node.kubernetes.io/unschedulable:NoSchedule
    Events:
    Type     Reason                  Age                   From               Message
    ----     ------                  ----                  ----               -------
    Normal   Scheduled               18m                   default-scheduler  Successfully assigned kube-system/calico-node-qpnh8 to node1
    Warning  FailedCreatePodSandBox  3m23s (x33 over 18m)  kubelet, node1     Failed create pod sandbox: rpc error: code = Unknown desc = failed pulling image "k8s.gcr.io/pause:3.1": Error response from daemon: Get https://k8s.gcr.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
    ```

    发现是数据节点的镜像拉取不下来，因此登录到数据节点通过 docker 手动拉取先。

    ```shell
    [root@node1 ~]# docker pull registry.aliyuncs.com/google_containers/pause:3.1
    3.1: Pulling from google_containers/pause
    67ddbfb20a22: Pull complete 
    Digest: sha256:f78411e19d84a252e53bff71a4407a5686c46983a2c2eeed83929b888179acea
    Status: Downloaded newer image for registry.aliyuncs.com/google_containers/pause:3.1
    [root@node1 ~]# docker images | grep pause
    registry.aliyuncs.com/google_containers/pause   3.1                 da86e6ba6ca1        6 years ago         742kB
    [root@node1 ~]# docker tag registry.aliyuncs.com/google_containers/pause:3.1 k8s.gcr.io/pause:3.1
    [root@node1 ~]# docker pull registry.aliyuncs.com/google_containers/kube-proxy:v1.15.12
    v1.15.12: Pulling from google_containers/kube-proxy
    6d63ffd718e0: Pull complete 
    20a2a24c2af9: Pull complete 
    ba37cbc6638b: Pull complete 
    Digest: sha256:937cc513f162062f8b2642de08ce6e3877cae5cf58e5276830796749e29cab42
    Status: Downloaded newer image for registry.aliyuncs.com/google_containers/kube-proxy:v1.15.12
    [root@node1 ~]# docker tag registry.aliyuncs.com/google_containers/kube-proxy:v1.15.12 k8s.gcr.io/kube-proxy:v1.15.12
    ```

    再次查看：

    ```shell
    [root@master ~]# kubectl get pod -A
    NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE
    kube-system   calico-kube-controllers-659f4b66fd-hg8tg   1/1     Running   0          23m
    kube-system   calico-node-gl8tz                          1/1     Running   0          23m
    kube-system   calico-node-qpnh8                          1/1     Running   0          23m
    kube-system   coredns-5c98db65d4-7qrs8                   1/1     Running   0          41m
    kube-system   coredns-5c98db65d4-mpvpr                   1/1     Running   0          41m
    kube-system   etcd-master                                1/1     Running   0          40m
    kube-system   kube-apiserver-master                      1/1     Running   0          40m
    kube-system   kube-controller-manager-master             1/1     Running   0          40m
    kube-system   kube-proxy-g6tqp                           1/1     Running   0          41m
    kube-system   kube-proxy-jfpz8                           1/1     Running   0          32m
    kube-system   kube-scheduler-master                      1/1     Running   0          40m
    [root@master ~]# kubectl get node
    NAME     STATUS   ROLES    AGE   VERSION
    master   Ready    master   42m   v1.15.5
    node1    Ready    <none>   33m   v1.15.5
    ```

<font color=red size=5>安装完成！！！</font>



   

