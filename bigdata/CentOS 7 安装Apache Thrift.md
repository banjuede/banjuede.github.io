# 1. Update the System
```
sudo yum -y update
```

# 2. Install the Platform Development Tools
```
sudo yum -y groupinstall "Development Tools"
```

# 3. Upgrade autoconf/automake/bison
```
sudo yum install -y wget
```

## 3.1 Upgrade autoconf
```
wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar xvf autoconf-2.69.tar.gz
cd autoconf-2.69
./configure --prefix=/usr
make
sudo make install
cd ..
```

## 3.2 Upgrade automake
```
wget http://ftp.gnu.org/gnu/automake/automake-1.14.tar.gz
tar xvf automake-1.14.tar.gz
cd automake-1.14
./configure --prefix=/usr
make
sudo make install
cd ..
```

## 3.3 Upgrade bison
```
wget http://ftp.gnu.org/gnu/bison/bison-2.5.1.tar.gz
tar xvf bison-2.5.1.tar.gz
cd bison-2.5.1
./configure --prefix=/usr
make
sudo make install
cd ..
```

# 4. Add Optional C++ Language Library Dependencies
## 4.1 Install C++ Lib Dependencies
```
sudo yum -y install libevent-devel zlib-devel openssl-devel
```

## 4.2 Upgrade Boost >= 1.53
```
wget http://sourceforge.net/projects/boost/files/boost/1.53.0/boost_1_53_0.tar.gz
tar xvf boost_1_53_0.tar.gz
cd boost_1_53_0
./bootstrap.sh
sudo ./b2 install
```

# 5. Build and Install the Apache Thrift IDL Compiler
```
git clone https://git-wip-us.apache.org/repos/asf/thrift.git
cd thrift
./bootstrap.sh
./configure --with-lua=no
make
```
> 在 `make` 这一步会发生一个错误 `g++: error: /usr/lib64/libboost_unit_test_framework.a: No such file or directory`，
> 错误原因是：`./configure` 的时候是默认编译32位的，不会在 `/usr/lib64/` 下产生文件
> 修改方法：先查找文件 `find / -name libboost_unit_test_framework.a`，比如在 `/usr/local/lib/libboost_unit_test_framework.a`，就可以做如下操作，`sudo ln -s /usr/local/lib/libboost_unit_test_framework.a /usr/lib64/libboost_unit_test_framework.a`，然后重新执行 `make`。
```
sudo make install
```
