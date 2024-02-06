# 云计算

## 什么是云计算 ？

云计算是一种通过 __计算机网络__ 以服务的方式提供 __动态可伸缩的虚拟化资源__ 的 __计算模式__。

按照服务层次分为：

- IaaS

- PaaS

- SaaS

按照部署方式分为：

- 公有云

- 私有云

- 部署云

这里需要注意两点：

1. 云计算是通过虚拟化技术实现，表现为虚拟化资源。

2. 云计算是一种按量付费的模式。

!!! tips "云计算通俗的解释"

    比如一台硬件服务器，有内存 20G、硬盘 500G、CPU 12 个，传统方式需要整台租用。但是我可能用不了那么多资源，于是有人通过虚拟化技术将内存、硬盘、CPU 抽象出资源池，这个过程就叫做 __资源虚拟化__。于是虚拟化得资源就可以随意使用了，20G 的内存租给 20 个人，每个人分 1G 内存使用，用完又回收到资源池内（ __动态可伸缩__ ）。所以你用 1G 内存就交 1G 内存的钱，用 10G 内存的钱，__这是一种线上费用计算模式，称之为云计算__。

!!! tips "生活的例子类比云计算"

    以前网吧很普遍，我们买不起整台电脑，但是我们可以用几块钱换取一小时租用电脑来上网，这就是一种按量付费的模式，只不过我们租用的是实体电脑；一旦我们将网吧的硬件资源虚拟化，搬到互联网来出租计算，完全可以叫做云网吧，这就是云计算。


## 为什么要用云计算

想象下面两种场景。

!!! tips "场景一：创业小公司"

    当一家小公司初期创业，用了一年时间，10 人团队投资 500W 开发了一款游戏，内测口碑很不错，准备上线，需要招聘一个运维来管理上线及维护。

    那么游戏运行需要服务器，如果买物理服务器，需要托管至 IDC 机房，防止断电。比如 10 台 *2w / 每台，托管 IDC 机房，8k / 年 / 每台，带宽 100M 每年 10w，总的成本就在38w。

    而如果买云服务器，一台云服务器我们选择高配，也才 800 一年，10 台也就 8000元，成本大大降低。对于初创公司非常友好。

    当这个游戏口碑直线上升，用户量激增时，又要买物理服务器、扩集群、增加占地面积，要是这款游戏黄了，这些物理服务器怎么办？二手甩卖太亏；而云服务器可以轻易扩缩容，需要就增加，不需要就减少。

    物理服务器买来还要装系统装软件；而云服务器买来就能用，缩短上线周期。


!!! tips "场景二：巨头大公司"

    如阿里、百度、京东，都有自己的云计算，这些大公司每逢节假日都有优惠促销活动。

    那么当天的用户量将是平时的 10 倍以上，如果 5 个集群可以抗住平时的流量访问，那么节日就得50个集群！这时候买物理服务器来得及吗？即使买来了，节日一过，50 个集群将严重资源浪费。

    如果是云服务器，就可以实现动态扩缩容，甚至将多余的计算资源出租赚钱，这就是为什么很多大公司都有自己的云计算，不仅自己用，还可以出租。
    

## 物理服务器与云服务器对比

| ——           | 物理服务器     | 云服务器      |
| ------------ | ------------- | ------------ |
| 成本          | 高  | 低 |
| 可伸缩        | 差  | 强 |
| 按照周期      | 长  | 短 |

## 云计算服务类型

### IaaS（Infrastruture as a Service）

IaaS 是 Infrastructure as a Service 的缩写，即 __基础设施即服务__

IaaS是一种云服务，提供基本的计算基础结构：__服务器（CPU），存储（内存、磁盘）和网络资源（ip与端口）__。通俗来说，就是把物理服务器资源，如CPU、内存、磁盘 __虚拟化__ ，放入一个资源池里，客户端按需申请使用。

如下图：

![IaaS]()

IaaS 服务可用于多种目的，从托管网站到分析大数据。客户可以在所获得的基础架构上安装和使用他们喜欢的任何操作系统和工具。IaaS的主要提供商包括 Amazon Web Services，Microsoft Azure 和Google Compute Engine。

### Pass（Platform as a Service）

PaaS 是 Platform as a Service 的缩写，即 __平台即服务__。

PaaS是指为运行时环境提供用于开发，测试和管理应用程序的云平台。

很好理解，__PaaS 就是在 IaaS 的基础上把运行时环境、中间件、操作系统也都给你装好了，而客户端就在 PaaS 上安装应用程序，生产个人数据__。用户只需要关注自己的业务逻辑，而不需要关注底层。PaaS服务的示例包括Heroku和Google App Engine。

如下图：

![PaaS]()

### Saas（Software as a Service）

SaaS 是 Software-as-a-Service 的缩写，即 __软件即服务__

SaaS 允许人们使用基于云的Web应用程序。这样就更方便了，__直接在 PaaS 的基础上把应用程序都安装好了，生成的数据也都在云服务器上，客户端只需要连接上网络就可以直接使用应用__。

如下图：

![SaaS]()

## 云计算部署类型

### 云服务

云服务提供商 __部署 IT 基础设施__ 并进行运营维护，将基础设施所承载的标准化、无差别的IT资源 __提供给公众客户__ 的服务模式，称为云服务。

分为:

- 公有云。

- 私有云。

- 混合云。

### 公有云

公有云的 __核心特征是基础设施所有权属于云服务商，云端资源向社会大众开放__，符合条件的任何个人或组织都可以租赁并使用云端资源，且无需进行底层设施的运维。公有云的优势是成本较低、无需维护、使用便捷且易于扩展，适应个人用户、互联网企业等大部分客户的需求。

公有云特点：

1. 成本低：不需要购买硬件或软件，只需要对使用的服务付费即可。

2. 不需要维护：维护由服务提供商提供。

3. 近乎无限制的缩放性：提供按需资源，可满足业务需求。

4. 高可靠性：具备众多服务器，确保免受故障影响。


### 私有云

云服务商为 __单一客户__ 构建IT基础设施，相应的 IT 资源仅供该客户 __内部员工__ 使用的产品交付模式。__私有云的核心特征是云端资源仅供某一客户使用，其他客户无权访问__。

由于私有云模式下的基础设施与外部分离，__因此数据的安全性、隐私性相比公有云更强，满足了政府机关、金融机构以及其他对数据安全要求较高的客户的需求__。

私有云的特点：

1. 灵活性更强。组织可自定义云环境以满足特定业务需求。

2. 控制力更强。资源不与其他组织共享，因此能获得更高的控制力以及更高的隐私级别。

3. 可伸缩性更强。与本地基础结构相比，私有云通常具有更强的可伸缩性。

### 混合云

用户同时使用公有云和私有云的模式。

- 一方面，用户在本地数据中心搭建私有云，处理大部分业务并存储核心数据。

- 另一方面，用户通过网络获取公有云服务，满足峰值时期的IT资源需求。

混合云能够在部署互联网化应用并提供最佳性能的同时，__兼顾私有云本地数据中心所具备的安全性和可靠性，并更加灵活地根据各部门工作负载选择云部署模式，因此受到规模庞大、需求复杂的大型企业的广泛欢迎__。

混合云的特定：

1. 控制力：组织可以针对需要低延迟的敏感资产或工作负载维护私有基础结构。

2. 灵活性：需要时可以用公有云中的其它资源。

3. 成本效益：成本效益：具备扩展至公有云的能力，因此可仅在需要时支付额外的计算能力。

4. 轻松使用：不需要费时即可过渡到云，因为可根据时间按照工作负载逐步迁移。


## 落地实践

如今云计算落地实践中，已经形成了一套有效的体系。

一家公司的传统业务如数据库、计算引擎、前端等，这样业务要上云，那么首先需要一个 IaaS，这个 IaaS 可以租用大公司已经搭建好的框架，也可以自己搭建 IaaS，如今自己搭建 IaaS 有三种主流工具：OpenStack、VmWare 和 RedHat，这三种方式各有千秋，而 OpenStack 在个人自学是最常见的。

OpenStack 是一个开源的云计算管理平台项目，是一系列软件开源项目的组合，它为私有云和公有云提供可扩展的弹性的云计算服务，它的核心项目（即 OpenStack 服务）组成：

- __计算（Compute）：Nova__。一套控制器，用于为单个用户或使用群组管理虚拟机实例的整个生命周期，根据用户需求来提供虚拟服务。负责虚拟机创建、开机、关机、挂起、暂停、调整、迁移、重启、销毁等操作，配置 CPU、内存等信息规格。

- __对象存储（Object Storage）：Swift__。一套用于在大规模可扩展系统中通过内置冗余及高容错机制实现对象存储的系统，允许进行存储或者检索文件。可为 Glance 提供镜像存储，为 Cinder 提供卷备份服务。

- __镜像服务（Image Service）：Glance__。一套虚拟机镜像查找及检索系统，支持多种虚拟机镜像格式（ISO、QCOW2、Raw、VMDK），有创建镜像、上传镜像、删除镜像、编辑镜像基本信息的功能。

- __身份服务（Identity Service）：Keystone__。为 OpenStack 其他服务提供身份验证、服务规则和服务令牌的功能，管理Domains、Projects、Users、Groups、Roles。

- __网络&地址管理（Network）：Neutron__。提供云计算的网络虚拟化技术，为 OpenStack 其他服务提供网络连接服务。为用户提供接口，可以定义 Network、Subnet、Router，配置 DHCP、DNS、VLAN。

- __块存储 (Block Storage)：Cinder__。为运行实例提供稳定的数据块存储服务，它的插件驱动架构有利于块设备的创建和管理，如创建卷、删除卷，在实例上挂载和卸载卷。

- __数据库服务（Database Service）：Trove__。为用户在 OpenStack 的环境提供可扩展和可靠的关系和非关系数据库引擎服务。

有了 OpenStack，并将其安装到集群中，集群是由多台物理服务器组成，OpenStack 将集群的硬件资源，如 CPU、内存、磁盘、网络虚拟化扔进资源池里提供给客户端使用，OpenStack 管理这些资源的申请、释放等一系列生命周期。

![OpenStack 各项目间的主要关系]()

这样我们就打造出一个 IaaS 层了。接下来利用 IaaS 层的资源创建出 __虚拟机__ 来，__虚拟机就像一台物理机一样有操作系统，也可以在上面安装应用__。

虚拟机的实现方式是在物理计算机上运行一个虚拟化软件层，称为虚拟机管理器（VMM），也称为 `hypervisor`。VMM 使得多个虚拟机实例可以在同一台物理计算机上运行，同时保证它们之间 __互相隔离__，以及对物理计算机资源的适当分配和管理。

![虚拟机的实现]()

这样带来的好处是：

- __资源隔离__：不同的业务可以在不同的虚拟机上运行，互不干扰，达到资源隔离。

- __动态伸缩__：当我们不需要业务了，直接删除虚拟机，释放资源即可，十分方便。需要多少资源，就创建对应资源的虚拟机，动态伸缩性好。

- __配置灵活__：一台物理机上可以运行多个虚拟机，多个虚拟机可以安装不同的操作系统，如 Linux、macos、Ubuntu。

- __易于迁移__: 如果物理主机 A 出现故障，A 上面的虚拟机可以迁移到物理主机 B 上去，这样不影响业务。


有了虚拟机后，我们就可以在虚机上安装相关应用了。__你会发现，在虚机上安装相关应用这一步和我们在物理机上安装应用好像没什么区别，这一步甚至根本不涉及到 PaaS 层__。

### 物理机与虚拟机

当我们在物理机部署时，需要安装操作系统，然后布置应用，如 redis，mysql，nginx，缺点很明显：

- 部署慢。

- 成本高。

- 资源浪费。

- 难以迁移和扩展。

- 可能会被限定硬件厂商。

由于物理机的都铎问题，于是出现了虚拟机，一个物理机上可以部署多个虚拟机，一个虚拟机上运行一个 app。但是虚拟机还是有局限性，我们依然要在虚拟机上安装应用，这一步花费的时间和物理机没有差别。__不仅如此，如果一个虚拟机上运行一个 app，这个 app 依赖的配置和 SDK 可能很少，但是我们却要为这个 app 创建出一个完整的操作系统来，这也太浪费资源了，这样的后果是物理主机创建不了多少个虚拟机就将资源耗尽__。


### 容器虚拟技术

物理机和虚拟机都有局限，那么有人就思考了，__安装应用需要依赖操作系统，一个操作系统可以安装多个应用__。如果我将应用依赖的操作系统抽取出来，只跑一个应用，__如 redis 和 mysql 两个数据库，对于操作系统的配置要求、SDK 依赖不一样，那么给 redis 抽取一个操作系统只包含 redis 独有的配置与 SDK 依赖，而给 mysql 抽取一个操作系统只包含 mysql 独有的配置与 SDK 依赖，那么我们就又实现了一层资源隔离，而且还节约资源了，因为不需要为一个 app 创建出完整的操作系统来__。

这就是容器虚拟技术的设想。app 就像运行在一个容器里面一样。

这一技术除了资源隔离和节约资源外确实没有太大进步。但是当结合 __Union File System（联合文件系统）技术后__，容器虚拟技术再也无可阻挡。在联合文件系统中，多个文件系统被按照特定的顺序进行层次化合并，形成一个虚拟的文件系统层次结构。当用户在这个虚拟的文件系统中访问文件或目录时，联合文件系统会根据预先设定的规则从各个底层文件系统中选择合适的文件或目录进行返回。在联合文件系统中，各个底层文件系统的文件和目录并不会被复制到新的文件系统中，而是通过符号链接或者其他方式进行引用，从而实现了文件系统的合并。

![容器虚拟技术]()

__容器虚拟技术带来的好处__：

- __节省资源__：容器相对于虚拟机，容器只需要具备 app 运行的配置和 SDK 即可，不需要创建完整的操作系统，使用的资源大大减少。

- __资源隔离__：比如一个虚拟机，跑一个 app，如 redis，没什么问题。但是我想跑不同版本的 redis，那么配置是不是可能冲突，环境是不是受到了污染 ？而如果换用容器技术，一个容器就是一个小操作系统，一个虚拟机上可以跑多个容器，那么轻易实现了跑不同版本的 redis，而且还资源隔离，删除容器即卸载了对应版本，是不是非常方便。

- __安装时间大大缩短和灵活移植__：一个容器运行了 redis，对应的系统配置和 SDK 依赖都是一样的，那么如果我能将这个容器导出成文件格式，然后再另一台虚拟机上导入运行起来，这样是不是就节省了安装时间呢？没错，容器虚拟技术结合 Union File System（联合文件系统）技术就是这么做的。这样带来的效率提升简直无可言喻。开发以容器虚拟技术开发应用后，将其打包导出，直接交给测试，测试在自己的环境上导入并运行，直接就可以完成测试，大大缩短了应用开发周期。而测试人员完成应用测试后，将其导出，直接就可以上线工业环境，缩短了业务上线周期。甚至将文件开源，建立容器镜像库，全世界的人都可以使用，安装应用只需要下载对应镜像即可快速安装，大大减少了学习成本。


### Docker 横空出世

其中容器虚拟化技术以 Docker 为首，Docker 是基于 Go 语言实现的云开源项目，Docker 的主要目标是“Build，Ship and Run Any App,Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的 APP（可以是一个 WEB 应用或数据库应用等等）及其运行环境能够做到“一次镜像，处处运行”。

![虚拟机化技术与容器虚拟化技术对比]()

1. 虚拟机技术需要在宿主机上安装虚拟化软件，然后在虚拟化软件上安装操作系统和应用程序。每个虚拟机都有自己的操作系统内核和资源管理器，因此虚拟机可以运行不同版本的操作系统和应用程序。容器虚拟化技术则不需要虚拟化软件，而是在宿主机的操作系统上运行，共享主机的操作系统内核。每个容器都有自己的用户空间，但共享主机的内核和系统资源。

2. 虚拟机技术提供了更高的隔离性和安全性，因为每个虚拟机都运行在独立的环境中，并且可以配置不同的安全策略和网络设置。容器虚拟化技术虽然也提供了一定的隔离性，但容器之间仍然共享主机的操作系统内核和系统资源，因此容器之间可能存在潜在的安全风险。

3. 虚拟机技术需要更多的资源，例如内存和处理器，因为每个虚拟机都需要运行一个完整的操作系统内核和资源管理器。容器虚拟化技术则可以共享主机的操作系统内核和系统资源，因此需要更少的资源，并且可以更高效地运行。

4. 虚拟机技术通常需要更长的启动时间，因为需要启动完整的操作系统和资源管理器。容器虚拟化技术启动时间更短，因为只需要启动容器的用户空间。

### PaaS 时代来临

容器虚拟化技术大行其道后，传统业务全部容器化，享受到容器化技术的便捷，同时问题也随之而来，当容器越来越多时，__容器的管理、编排成了大难题__。

容器的生命周期如何管理，容器故障如何监控，相互有依赖关系的容器如何部署等等问题。

人们发现，__需要借助一个平台来管理我们的容器__，PaaS 时代随之来临。PaaS 平台其中以 Kubernetes 为代表。Kubernetes，简称 K8s，是一个开源的、用于管理云平台中多个主机上的容器化的应用平台，提供了应用部署，规划，更新，维护的一种机制。Kubernetes 可以自动化应用程序的部署、扩展和管理，并提供了一种通用的方式来管理容器化应用程序。它支持多种容器运行时，包括 Docker，可以运行在公共云、私有云和混合云环境中。

![pass-k8s]()

### 业界实践

搭建 IaaS 后，用于创建虚拟机，在虚拟机上部署 PaaS，用于管理同时部署在虚拟机上的容器，这就是业界普遍的云计算实践。

![docker-k8s]()

当然我们也可以直接在物理服务器上构建 PaaS 集群，不需要再创建虚拟机。这种方式称为裸机部署模式，这是未来云计算发展的另一种趋势，好处就在于除去了虚拟机这一层，平台性能可以提升 30%。

## 动手实践

上面是业界普遍的云计算实践：__搭建 IaaS 后，用于创建虚拟机，在虚拟机上部署 PaaS，用于管理同时部署在虚拟机上的容器__。

但是对于自己学习而言，搭建一个 IaaS 成本太高了，一是没那么多资源，二是安装费时间，所以只要能创建虚拟机，满足学习需求即可。

准备如下：

- [CentOS 7.9 镜像文件](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)
- VMware Workstation Pro

### 创建 master 虚拟机

1. 打开 VMware Workstation Pro，点击【新建虚拟机】

    ![新建虚拟机](https://shichuan-hao.github.io/images/static/cloud/1s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择虚拟机硬件兼容性](https://shichuan-hao.github.io/images/static/cloud/2s-in-creating-a-master-virtual-machine.png)

    这里提前加载好我们下载的 iso 文件。
    
    ![新建虚拟机向导-安装客户机操作系统](https://shichuan-hao.github.io/images/static/cloud/3h-step-in-creating-a-master-virtual-machine.png)

    这里取个名字喜欢的名字，并且指定好存放路径，方便查找。

    ![新建虚拟机向导-命名虚拟机](https://shichuan-hao.github.io/images/static/cloud/4s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-处理器配置](https://shichuan-hao.github.io/images/static/cloud/5s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-虚拟机内存设置](https://shichuan-hao.github.io/images/static/cloud/6s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-网络类型](https://shichuan-hao.github.io/images/static/cloud/7s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择 I/O 控制类型](https://shichuan-hao.github.io/images/static/cloud/8s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择磁盘类型](https://shichuan-hao.github.io/images/static/cloud/9s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-选择磁盘](https://shichuan-hao.github.io/images/static/cloud/10s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘容量](https://shichuan-hao.github.io/images/static/cloud/11s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/12s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/13s-in-creating-a-master-virtual-machine.png)


    最后启动虚拟机，就开始安装操作系统了。

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/14s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/15s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/16s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/17s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/18s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/19s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/20s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/21s-in-creating-a-master-virtual-machine.png)
    
    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/22s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/23s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/24s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/25s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/26s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/27s-in-creating-a-master-virtual-machine.png)

    ![新建虚拟机向导-指定磁盘文件](https://shichuan-hao.github.io/images/static/cloud/28s-in-creating-a-master-virtual-machine.png)
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
