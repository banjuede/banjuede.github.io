# 从Ambari 2.4.0以后开始支持自定义服务的升级了，以下为自定义服务升级的基本思路：
> 貌似和Ambari 2.4.0之前有点不一样，之前是把服务目录放在stack/services目录下，现在是要把所有custom service目录放在resources/extension目录下，可以有不同版本的extension，不同的extension版本在不同的文件夹下，并设置可支持的HDP版本，最后custom service不同的版本链接不同的stack，这样建立连接以后custom service的升级就和stack的升级关联到了一起，再去升级stack时就可以升级custom service了。

## 具体步骤如下：
> 1. 安装配置启动ambari
> 2. 创建集群
> 3. 把设计好的extensions目录复制到resource目录下，然后重启ambari
> 4. 创建custom service 和 current installed stack 之间的连接

## 测试
> 1. 安装配置ambari
> 2. 复制extension目录到resource目录下
> 3. 启动ambari
> 4. 创建custom service 和 want to install stack 之间的连接
> 5. 重启ambari
