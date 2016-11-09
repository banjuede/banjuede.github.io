1. 安装Gradle
> **CentOS下安装**
> [安装Gradle](https://docs.gradle.org/current/userguide/installation.html?_ga=1.100105630.1438858406.1473148007)
> 下载安装包以后解压并配置环境变量(不罗嗦了)
> **Ubuntu下安装**
> `sudo apt-get install -f gradle`
>
> **测试Gradle是否安装成功**
> `gradle -v`

2. 使用Gradle编译
> `gradle build`
> gradle 会自动下载依赖包，并把源码编译成jar包
> 当然也可以在`build.gradle`文件中修改依赖