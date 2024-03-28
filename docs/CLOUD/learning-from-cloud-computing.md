# 动手实践

对于自己学习而言，搭建一个 IaaS 成本太高了，一是没那么多资源，二是安装费时间，所以只要能创建虚拟机，满足学习需求即可。

准备如下：

- [CentOS 7.9 镜像文件](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)
- VMware Workstation Pro

## 创建 master 虚拟机

打开 VMware Workstation Pro，点击【新建虚拟机】

<figure markdown="span">

  ![新建虚拟机](https://shichuan-hao.github.io/images/cloud/vmware-virtual/1s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 1：新建虚拟机想到</figcaption>

  ![新建虚拟机向导-选择虚拟机硬件兼容性](https://shichuan-hao.github.io/images/cloud/vmware-virtual/2s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 2：新建虚拟机向导 - 选择虚拟机硬件兼容性</figcaption>

</figure>

这里提前加载好下载的 iso 文件。

<figure markdown="span">

  ![新建虚拟机向导-安装客户机操作系统](https://shichuan-hao.github.io/images/cloud/vmware-virtual/3h-step-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 3：新建虚拟机向导 - 安装客户机操作系统</figcaption>

</figure>

这里取个名字喜欢的名字，并且指定好存放路径，方便查找。

<figure markdown="span">

  ![新建虚拟机向导-命名虚拟机](https://shichuan-hao.github.io/images/cloud/vmware-virtual/4s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 4：新建虚拟机向导 - 命名虚拟机</figcaption>

  ![新建虚拟机向导-处理器配置](https://shichuan-hao.github.io/images/cloud/vmware-virtual/5s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 5：新建虚拟机向导 - 处理器配置</figcaption>

  ![新建虚拟机向导-虚拟机内存设置](https://shichuan-hao.github.io/images/cloud/vmware-virtual/6s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 6：新建虚拟机向导 - 虚拟机内存设置</figcaption>

  ![新建虚拟机向导-网络类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/7s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 7：新建虚拟机向导 - 网络类型</figcaption>

  ![新建虚拟机向导-选择 I/O 控制类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/8s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 8：新建虚拟机向导 - 选择 I/O 控制类型</figcaption>

  ![新建虚拟机向导-选择磁盘类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/9s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 9：新建虚拟机向导 - 选择磁盘类型</figcaption>

  ![新建虚拟机向导-选择磁盘](https://shichuan-hao.github.io/images/cloud/vmware-virtual/10s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 10：新建虚拟机向导 - 选择磁盘</figcaption>

  ![新建虚拟机向导-指定磁盘容量](https://shichuan-hao.github.io/images/cloud/vmware-virtual/11s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 11：新建虚拟机向导 - 指定磁盘容量</figcaption>

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/12s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 12：新建虚拟机向导 - 指定磁盘文件</figcaption>

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/13s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 13：新建虚拟机向导 - 指定磁盘文件</figcaption>
</figure>

最好修改路径。

最后启动虚拟机，就开始安装操作系统了。

<figure markdown="span">

  ![新建虚拟机向导-命名虚拟机](https://shichuan-hao.github.io/images/cloud/vmware-virtual/14s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 14</figcaption>

  ![新建虚拟机向导-处理器配置](https://shichuan-hao.github.io/images/cloud/vmware-virtual/15s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 15</figcaption>

  ![新建虚拟机向导-虚拟机内存设置](https://shichuan-hao.github.io/images/cloud/vmware-virtual/16s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 16</figcaption>

  ![新建虚拟机向导-网络类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/17s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 17</figcaption>

  ![新建虚拟机向导-选择 I/O 控制类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/18s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 18</figcaption>
</figure>

顺便设置一下用户和密码。


### master 虚拟机设置

安装完成后，保持虚拟机处于关机状态。

<figure markdown="span">

  ![新建虚拟机向导-选择磁盘类型](https://shichuan-hao.github.io/images/cloud/vmware-virtual/19s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 19</figcaption>

  ![新建虚拟机向导-选择磁盘](https://shichuan-hao.github.io/images/cloud/vmware-virtual/20s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 20</figcaption>

</figure>

### 修改虚拟机 ip 为静态 ip

创建完成后，由于是刚刚创建好的虚拟机，输入 `ip a s` 命令可以看到 ens33 网卡 ip 没有绑定，是通过 dhcp 自动获取 ip 的，当前是没有办法连接到外网的。

并且使用 dhcp 会造成每次虚拟机重启改变 ip 地址改变，非常不方便，因此要让 ip 固定下来。 

### 采用 NAT 模式连接

首先，设置虚拟机中 NAT 模式的选项，打开 vmware，点击 “编辑” 下的 “虚拟机网络编辑器”。
<figure markdown="span">

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/23s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 21</figcaption>

</figure>

通过图 23 中红框 NAT 设置和 DHCP 设置可以找到 **NETMASK(子网掩码)**、**GATEWAY(网关)**、**IPADDR(ip 地址)**，记录下来，之后会用到。

<figure markdown="span">
  
  ![新建虚拟机向导-选择磁盘](https://shichuan-hao.github.io/images/cloud/vmware-virtual/24s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 22</figcaption>

</figure>

**DHCP 设置这里要注意，“起始 ip 地址”和“结束 ip 地址”已经规定了我们虚拟机 ip 的范围了**。如我们可以给虚拟机分配固定 ip 为 192.168.80.130

<figure markdown="span">

  ![新建虚拟机向导-指定磁盘容量](https://shichuan-hao.github.io/images/cloud/vmware-virtual/25s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 23</figcaption>

</figure>

### 修改虚拟机网卡配置文件

完成后，打开 centos，进入 root 模式，

```shell
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

在文件中添加 **NETMASK（子网掩码）**，**GATEWAY（网关）**，**IPADDR（ip地址）**，将刚才记录的地址写上，并将 ONBOOT 改为 yes（此变量控制网卡能否上网），我将 BOOTPROTO 设置为 none，设置为 static 也可以。`DNS1=8.8.8.8`，`DNS2=114.114.114.114`，`DNS3=116.116.116.116`


<figure markdown="span">

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/26s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 24</figcaption>

</figure>

```shell
ONBOOT=yes
NETMASK=255.255.255.0
IPADDR=192.168.18.129
GATEWAY=192.168.18.2
DNS1=8.8.8.8
DNS2=114.114.114.114
DNS3=116.116.116.116
```

!!! warning

    注意，原有内容不要删除。


而后，使用命令 `service network restart` 重启网络服务，使配置生效。

接着，ping `www.baidu.com`，ping 通说明我们配置完毕。

<figure markdown="span">

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/27s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 25</figcaption>

</figure>

最后检查绑定 ip。

<figure markdown="span">

  ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/cloud/vmware-virtual/27s-in-creating-a-master-virtual-machine.png){ width="600"}
  <figcaption>图 26</figcaption>

</figure>

可以发现，目前虚拟机已经绑定为我指定的 ip 了，这样即便是重启也不会改变虚拟机的 ip 了。

## 创建虚拟机：node 节点

创建一个数据节点 node1，用于后续学习 docker、k8s，方法参照以上 master 节点创建方法。不过数据节点性能要求没那么高，__一个 cpu 一个核心，1G 内存，10G 磁盘即可__。

## 配置节点信息

### 修改主机名

```shell
hostnamectl set-hostname master # master节点
hostnamectl set-hostname node1 # 数据节点

systemctl restart network
```

### 添加域名

```shell
vi /etc/hosts
hostnamectl set-hostname node1 # 数据节点

192.168.18.130 master
192.168.18.131 node1
```
两个节点都要添加。

### 验证，尝试两个节点互ping

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

## VMWare 创建虚拟机三种网络模式

### 桥接模式（bridged networking）

在桥接模式下，VMWare 虚拟出来的操作系统就像是局域网中一台独立的主机，它能够访问网内任何一台机器。通俗讲如同局域网中新增了一台主机。

在桥接模式下，你必须手工为虚拟系统配置 IP 地址、子网掩码，并且还要和宿主机器处于同一网段，这样虚拟系统才干和宿主机器进行通信。

配置好网关和DNS的地址后。才能实现通过局域网的网关或路由器访问互联网。

桥接模式：虚机和物理主机可通信，可与外网通信。

缺点：如同局域网中新增了一台主机，每台主机都要分配 ip，对于小局域网来说没有问题，但是一旦规模增大，ip 资源就不够了。

### 仅主机模式（Host-Only Mode）

仅让虚拟机的系统之间与物理主机通信，不能访问外网，在真机中对应的物理网卡是 VMnet1。

缺点：不能访问外网，用来做实验没有什么意义。但是可以用在网络安全要求高的场景。

### NAT 模式（network address translation）

这种模式就是让虚拟系统借助 NAT(网络地址转换)功能，通过宿主机器所在的网络来访问公网。NAT模式就是介于桥接模式与主机模式之间，解决了两者的缺点。

也就是说，使用 NAT 模式能够实现在虚拟的系统里面直接访问互联网。NAT 模式下的虚拟系统的 TCP/IP 配置信息是由 VMnet8(NAT) 虚拟网络的 DHCPserver 提供的，无法进行手工改动，因此虚拟系统也就无法和本局域网中的其它真实主机进行通讯。采用 NAT 模式最大的优势是虚拟系统接入互联网很容易。仅仅需要宿主机器能访问互联网，不需配置 IP 地址，子网掩码，网关。可是 DNS 地址还是要依据实际情况配的。

NAT 模式：虚机可和外网通信，NAT 模式就很好的解决了桥接模式资源不足的问题，同时也解决了主机模式不能上网的问题。

使用 NAT 模式，会创建一块名为 VMnet8 的虚拟网卡，相当于交换机，控制了虚拟机局域网互联，然后 VMnet8 与本地连接建立关系，保证虚机能访问外网。

![VMWare NAT 网络模式](https://shichuan-hao.github.io/images/cloud/vmware-virtual/vmware-nat.png)