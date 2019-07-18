---
title: CentOS 6.6下源码编译安装Python
date: 2015-11-19 20:35:12
tags: [Linux, Python]
categories: [学习笔记]
---

### 准备安装模块
```bash
yum groupinstall "Development tools"
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel
```

### Python3.4 安装下载代码 configure → make → make altinstall
```bash
 cd /usr/local/src
 wget https://www.python.org/ftp/python/3.4.3/Python-3.4.3.tgz
 tar -zxvf Python-3.4.3.tgz 
 cd Python-3.4.3
 ./configure --prefix=/usr/local/python
 make && make altinstall
 ```

### 安装Python3.4 ：公用库的安装
```bash
 echo "/usr/local/python/lib" >> /etc/ld.so.conf
 ldconfig
```
### 安装Python3.4： 4. /usr/local/bin/
```bash
 ln -s /usr/local/python/bin/python3.4 /usr/local/bin/python
```

### 安装Python3.4  ：5. 确认是否安装正确
```bash
 /usr/local/python/bin/python3.4 -V
 python -V
```

### 安装Python3.4  Easy_Install
```bash
 cd /usr/local/src
 wget https://pypi.python.org/…/s/setuptools/setuptools-18.0.1.zip
 unzip setuptools-18.0.1.zip
 cd setuptools-18.0.1
 /usr/local/bin/python setup.py install
 ln -s /usr/local/python/bin/easy_install /usr/local/bin/easy_install
```

### 安装Python3.4 ：Pip
```bash
 /usr/local/bin/easy_install pip
 ln -s /usr/local/python/bin/pip /usr/local/bin/pip
```

### 安装Python3.4： Virtualenv
```bash
 pip install virtualenv
 ln -s /usr/local/python/bin/virtualenv /usr/local/bin/virtualenv
```

### 安装Python3.4 ：Virtualenvwrapper
```bash
 pip install virtualenvwrapper
```

### 安装Python3.4 ：参数设定$ 
```bash
vim ~/.bashrc
if [ -f /usr/local/python/bin/virtualenvwrapper.sh ]; then
export WORKON_HOME=$HOME/.virtualenvs
source /usr/local/python/bin/virtualenvwrapper.sh
fi
```