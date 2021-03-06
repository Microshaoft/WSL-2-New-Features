# WSL-2-New-Features

### WSL 2 Windows 10 下目录
```sh
\\wsl$\Ubuntu\
```
### Windows 下先备份，修改 WSL 2 Windows 10 下文件并保存
```sh
"\\wsl$\Ubuntu\etc\apt\sources.list"
```
内容修改为
```
deb https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb-src https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
```

### 回到 WSL 2 执行如下命令报错
```sh
sudo  apt-get update
```
报错如下：
```sh
Err:2 https://mirrors.aliyun.com/debian-security bullseye-security InRelease
  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 112695A0E562B32A NO_PUBKEY 54404762BBB6E853
```
修复命令如下：
```sh
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 112695A0E562B32A
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 54404762BBB6E853
```

### 安装SSH,子系统间的通信,因为系统不同还是需要安装SSH服务器, unzip 和 curl 或 wget 等组件
```sudo apt-get install openssh-server unzip curl```

### 安装SSH后,系统并不能访问本机的系统的端口做通信,还需要配置一个SSH服务器的配置文件
```sudo nano /etc/ssh/sshd_config```

### 分别找到如下配置项做修改,修改后的内容如下:
```UsePAM no```

```UsePrivilegeSeparation no```

```PasswordAuthentication yes```

```#PermitRootLogin prohibit-password```

```PermitRootLogin yes```

```Port 2222```

### generate SSH keys for the SSH instance:
```sudo ssh-keygen -A```

### 最后重启下SSH服务
```sudo service ssh --full-restart```

### 每次启动Bash进程时都需要重新启动SSH Service
```sudo service ssh start```

### Install Docker Engine
https://docs.docker.com/engine/install/ubuntu/
https://www.docker.org.cn/docker/docker-206.html

1.Update the apt package index and install packages to allow apt to use a repository over HTTPS:
```sh
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2.Add Docker’s official GPG key:
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3.Use the following command to set up the stable repository. To add the nightly or test repository, add the word nightly or test (or both) after the word stable in the commands below. Learn about nightly and test channels.
```sh
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
4.Update the apt package index, and install the latest version of Docker Engine and containerd, or go to the next step to install a specific version:
```sh
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```
