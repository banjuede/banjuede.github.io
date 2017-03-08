## Ubuntu下访问Google会出现私密链接的问题

原因是没有导入GoAgent的证书

1. 安装导入证书工具
```
sudo apt-get install libnss3-tools
```
2. 导入证书
```
sudo certutil -d sql:$HOME/.pki/nssdb -A -t TC -n "goagent" -i ~/Dev/XX-Net-3.3.0/data/gae_proxy/CA.crt
```

## 后台运行
```
xx_net.sh start/stop/restart
```


## 开机自启
在`/etc/rc.local`中添加一行：
```
sudo xxnet安装目录/xx_net.sh start
```

其他具体操作请看：[github地址](https://github.com/XX-net/XX-Net)