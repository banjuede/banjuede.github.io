从Ambari 2.4以后添加自定义服务有了多种方式，如官网文档所说：
>```
> Custom services in Apache Ambari can be packaged and installed in many ways.  Ideally, they should all be packaged and installed in the same manner.  This document describes how to package and install custom services using Extensions and Management Packs.  Using this approach, the custom service definitions do not get inserted under the stack versions services directory.  This keeps the stack clean and allows users to easily see which services were installed by which package (stack or extension).
>```

有下列三种方式：
>```
> 1. 直接把 Service JOBSERVER 复制到 /var/lib/ambari-server/resource/stacks/HDP/2.5/services 目录下，然后 ambari-server restart
> 2. 把设计好的 extensions 目录复制到 /var/lib/ambari-server/resource 目录下，然后复制到 ambari-agent 的所有节点的 /var/lib/ambari-agent/cache 目录下，然后 ambari-server restart
> 3. 使用 Management Packs 安装（Ambari2.4 目前还不支持）
>```
