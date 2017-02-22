 **1. Mesos的地位**：
> Mesos可以认为是资源的中间商，Mesos是第一个可以在多个框架之间共享资源的集群调度器。

 **2. Mesos的设计理念**：
> Mesos做为数据中心的内核，需要为大量不同类型的负载提供服务，没有一个调度器可以满足所有这些框架的需求，为了应对这个问题，Mesos的设计原则是：资源分配和任务调度的分离。
> Mesos master 负责决定分配给每个框架多少资源，任务调度器负责如何使用这些资源，这取决于每个框架的调度器如何根据自身需求去实现。也可以这样理解Mesos的设计理念：Mesos在各个框架间进行粗粒度的资源分配，每个框架根据自身任务的特点进行细粒度的任务调度。
 
  **3. CentOS 7.1安装Mesos**
 > 使用yum安装很方便，这里就使用yum安装Mesos，手动安装参见[官网安装步骤](http://mesos.apache.org/gettingstarted/)，这里默认已经安装了zookeeper。
 > 
 > **节点设计**：
 > node1：mesos-master
 > node2：mesos-slave
 > node3：mesos-slave
 > 
 > **①每个节点添加mesosphere的yum源**
 > `rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-1.noarch.rpm`

 > **②每个节点安装mesos**
 > `yum -y install mesos marathon`

 > **③每个节点配置mesos**
 > 配置zookeeper的地址
 > `vim /etc/mesos/zk`
 >`zk://172.18.2.94:2181,172.18.2.95:2181,172.18.2.96:2181/mesos`

 >**④mesos-master节点配置**
 >`vi /etc/mesos-master/quorum`
>(在里面添加一个数字，数字大小不小于master的数量除以2，这里测试一个master，填1就可以)
>`vi /etc/mesos-master/ip`
>(添加master的ip，默认是127.0.0.1，只做显示用)
>`vi /etc/mesos-master/hostname`
>(添加master的hostname，默认为localhost，主要在mesos集群间使用，不是机器的hostname，只做显示用)

> **⑤mesos-slave节点配置**
> 如果要运行dockers，则要配置docker启动
> （**Note：slave节点都必须安装docker并且docker的版本必须一致，否则下面的配置会导致slave节点启动不了**）
 >`echo 'docker,mesos' > /etc/mesos-slave/containerizers`
>`echo '5mins' > /etc/mesos-slave/executor_registration_timeout`

>**⑥服务配置**
>由于yum安装会把mesos-master和mesos-slave添加到开机自启动，所以：
>
> mesos-master节点需要停止slave服务并关闭开机启动
> `systemctl stop mesos-slave.service`
> `systemctl disable mesos-slave.service`
>
> mesos-slave节点需要停止master服务并关闭开机启动
> `systemctl stop mesos-master.service`
> `systemctl disable mesos-master.service`

>**⑦启动服务**
>`systemctl restart mesos-master.service`
>`systemctl restart mesos-slave.service`
 
 > 启动服务后访问WebUI：`http://mesos-master:5050/`
> ![WebUI](http://upload-images.jianshu.io/upload_images/1382065-fd4861cd5dc89213.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)