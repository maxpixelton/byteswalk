# 快速安装 Docker + Kuerbernetes


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
            "registry-mirrors": ["https://4u6ad4xd.mirror.aliyuncs.com"]
            }
            EOF
            sudo systemctl daemon-reload
            sudo systemctl enable docker
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

Kubelet 是 Kubernetes 集群中的一个核心组件，它是每个节点上的代理，负责维护节点上容器的生命周期。

- Kubelet 能够根据集群的配置和调度决策，启动、停止、重启和销毁容器，并监控它们的状态。

- Kubelet 还负责管理容器和节点之间的网络，确保容器能够相互通信，同时也支持网络插件的扩展。

kubectl 是 Kubernetes 的命令行界面工具，它是管理和操作 Kubernetes 集群的主要方式之一。

- 使用 kubectl，用户可以通过简单的命令行指令来管理 Kubernetes 集群中的各种资源对象，如 Pod、Service、Deployment、ConfigMap 等等。


安装过程如下。

1. 配置 yum 源。

2. 更新 yum 索引。

3. 列出所有可安装的版本。

4. 安装指定版本。

5. 前置操作

6. 设置 kubelet 开机启动

7. 生成 master 节点配置 kubeadm 的初始化文件

8. 提前下载需要的镜像

9. 安装前检查

10. 初始化 kubeadm

11. 检查安装结果



```shell
Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.80.130:6443 --token 5f11jg.jblqq9kop6iav7xx \
    --discovery-token-ca-cert-hash sha256:36e5705d03ba44380d6c6a9db0925c9370c24e756b2beca1692735959c9fcb6d 
```