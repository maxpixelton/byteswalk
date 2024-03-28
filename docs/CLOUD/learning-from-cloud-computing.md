# 动手实践

上面是业界普遍的云计算实践：__搭建 IaaS 后，用于创建虚拟机，在虚拟机上部署 PaaS，用于管理同时部署在虚拟机上的容器__。

但是对于自己学习而言，搭建一个 IaaS 成本太高了，一是没那么多资源，二是安装费时间，所以只要能创建虚拟机，满足学习需求即可。

准备如下：

- [CentOS 7.9 镜像文件](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)
- VMware Workstation Pro

### 创建 master 虚拟机

1. 打开 VMware Workstation Pro，点击【新建虚拟机】

    ![新建虚拟机](https://shichuan-hao.github.io/images/cloud/1s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择虚拟机硬件兼容性](https://shichuan-hao.github.io/images/cloud/2s-in-creating-a-master-virtual-machine.png)

    这里提前加载好我们下载的 iso 文件。
    
    ![新建虚拟机向导-安装客户机操作系统](https://shichuan-hao.github.io/images/cloud/3h-step-in-creating-a-master-virtual-machine.png)

    这里取个名字喜欢的名字，并且指定好存放路径，方便查找。

    ![新建虚拟机向导-命名虚拟机](https://shichuan-hao.github.io/images/cloud/4s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-处理器配置](https://shichuan-hao.github.io/images/cloud/5s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-虚拟机内存设置](https://shichuan-hao.github.io/images/cloud/6s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-网络类型](https://shichuan-hao.github.io/images/cloud/7s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择 I/O 控制类型](https://shichuan-hao.github.io/images/cloud/8s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择磁盘类型](https://shichuan-hao.github.io/images/cloud/9s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择磁盘](https://shichuan-hao.github.io/images/cloud/10s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘容量](https://shichuan-hao.github.io/images/cloud/11s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/12s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/13s-in-creating-a-master-virtual-machine.png)


    最后启动虚拟机，就开始安装操作系统了。

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/14s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/15s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/16s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/17s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/18s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/19s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/20s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/21s-in-creating-a-master-virtual-machine.png)
    
    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/22s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/23s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/24s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/25s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/26s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/27s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/28s-in-creating-a-master-virtual-machine.png)
2. 

### 创建数据节点

创建一个数据节点 node1，用于后续学习 docker、k8s，方法参照以上 master 节点创建方法。不过数据节点性能要求没那么高，__一个 cpu 一个核心，1G 内存，10G 磁盘即可__。

### 配置

1. 修改主机名

    ```shell
    hostnamectl set-hostname master # master节点
    hostnamectl set-hostname node1 # 数据节点

    systemctl restart network
    ```

2. 添加域名

    ```shell
    vi /etc/hosts
    hostnamectl set-hostname node1 # 数据节点

    192.168.18.130 master
    192.168.18.131 node1
    ```

    两个节点都要添加。

3. 验证，尝试两个节点互ping

    ```shell
    [root@master mtuser]# ping node1
    PING node1 (192.168.18.130) 56(84) bytes of data.
    64 bytes from node1 (192.168.18.130): icmp_seq=1 ttl=64 time=1.93 ms
    64 bytes from node1 (192.168.18.130): icmp_seq=2 ttl=64 time=3.09 ms

    [root@node1 mtuser]# ping master
    PING master (192.168.18.129) 56(84) bytes of data.
    64 bytes from master (192.168.18.129): icmp_seq=1 ttl=64 time=1.05 ms
    64 bytes from master (192.168.18.129): icmp_seq=2 ttl=64 time=1.70 ms
    ```

### VMWare 创建虚拟机三种网络模式

#### 桥接模式（bridged networking）

在桥接模式下，VMWare 虚拟出来的操作系统就像是局域网中一台独立的主机，它能够访问网内任何一台机器。通俗讲如同局域网中新增了一台主机。

在桥接模式下，你必须手工为虚拟系统配置 IP 地址、子网掩码，并且还要和宿主机器处于同一网段，这样虚拟系统才干和宿主机器进行通信。

配置好网关和DNS的地址后。才能实现通过局域网的网关或路由器访问互联网。

桥接模式：虚机和物理主机可通信，可与外网通信。

缺点：如同局域网中新增了一台主机，每台主机都要分配 ip，对于小局域网来说没有问题，但是一旦规模增大，ip 资源就不够了。

#### 仅主机模式（Host-Only Mode）

仅让虚拟机的系统之间与物理主机通信，不能访问外网，在真机中对应的物理网卡是 VMnet1。

缺点：不能访问外网，用来做实验没有什么意义。但是可以用在网络安全要求高的场景。

#### NAT 模式（network address translation）

这种模式就是让虚拟系统借助 NAT(网络地址转换)功能，通过宿主机器所在的网络来访问公网。NAT模式就是介于桥接模式与主机模式之间，解决了两者的缺点。

也就是说，使用 NAT 模式能够实现在虚拟的系统里面直接访问互联网。NAT 模式下的虚拟系统的 TCP/IP 配置信息是由 VMnet8(NAT) 虚拟网络的 DHCPserver 提供的，无法进行手工改动，因此虚拟系统也就无法和本局域网中的其它真实主机进行通讯。采用 NAT 模式最大的优势是虚拟系统接入互联网很容易。仅仅需要宿主机器能访问互联网，不需配置 IP 地址，子网掩码，网关。可是 DNS 地址还是要依据实际情况配的。

NAT 模式：虚机可和外网通信，NAT 模式就很好的解决了桥接模式资源不足的问题，同时也解决了主机模式不能上网的问题。

使用 NAT 模式，会创建一块名为 VMnet8 的虚拟网卡，相当于交换机，控制了虚拟机局域网互联，然后 VMnet8 与本地连接建立关系，保证虚机能访问外网。

![VMWare NAT 网络模式]()