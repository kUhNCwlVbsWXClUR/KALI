# KALI
 Configuration About KALI Aystem For Development And Entertainment
# kali 系统配置


## step 1.

- 创建 sudo 用户

```bash
sudo root
adduser username
usermod -aG sudo userename
su username
logout

```

## step 2.
- 更改 kali 源

```bash
vim /etc/apt/source.list

```

- 内容如下

```angular2html
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

```

- update 

```bash
sudo apt-get update

```
## step 3.
- 安装 sougou 输入法
- 下载安装包 [下载链接](https://pinyin.sogou.com/linux/)
- 安装

```bash
sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb
sudo apt-get -f install
sudo reboot
fcitx-config-gtk3  # 添加安装好的搜狗输入法. 可以根据个人爱好，设置触发输入法的按键， 个人喜欢用shift键

```

## step 4.
- 安装 chrome 浏览器

```bash
sudo dpkg -i google-chrome-stable_67.0.3396.99-1_amd64.deb
sudo apt-get -f install

```

## step 5.
- 安装 pycharm
- [官网地址](http://www.jetbrains.com/pycharm/)

```bash
sudo mkdir /usr/local/charm
sudo tar -xvf pycharm-professional-2018.1.4.tar.gz -C /usr/local/charm/
ln -s /usr/local/charm/pycharm-2018.1.4/bin/pycharm.sh /usr/bin/charm
charm # start pycharm
# 记得安装vim和markdown插件 [我的最爱!]

```

## step 6.
- 安装 proxy

```bash
sudo pip install shadowsocks==2.8.2
sudo apt-get install polipo

```

- 修改文件

```bash
sudo vim /usr/local/lib/python2.7/dist-packages/shadowsocks-2.8.2-py2.7.egg/shadowsocks/crypto/openssl.py
```

<p>
根据关键字CTX_clean 有两处地方， 替换成CTX_reset
</p>

- 配置 shadowsocks

```bash
sudo vim /etc/shadowsocks.json
```

```angular2html
{
    "server":"input your shadowsocks server ip",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"input your shadowsocks server password",
    "timeout":300,
    "method":"aes-256-cfb"
}

```

- 配置 polipo

```bash
sudo vim /etc/polipo/config
```

```angular2html
logSyslog = true
logFile = /var/log/polipo/polipo.log
      
proxyAddress = "0.0.0.0"  
socksParentProxy = "127.0.0.1:1080"  
socksProxyType = socks5  
chunkHighMark = 50331648  
objectHighMark = 16384  
serverMaxSlots = 64  
serverSlots = 16  
serverSlots1 = 32

```


## step 7.
```bash
apt-get update
apt-get install -y virtualbox-guest-x11
reboot
sudo apt-get install virtualbox
```

## step 8.
# proxy file

```sh
# /usr/bin/proxy
start(){
    sslocal -c /etc/shadowsocks.json -d start --pid-file ~/.ss.pid --log-file ~/.ss.log
    gsettings set org.gnome.system.proxy.http host 'localhost'
    gsettings set org.gnome.system.proxy.http port 8123
    gsettings set org.gnome.system.proxy mode 'manual'
}
stop(){
    sslocal -c /etc/shadowsocks.json -d stop --pid-file ~/.ss.pid --log-file ~/.ss.log
    gsettings set org.gnome.system.proxy mode 'none'
}
restart(){
    stop
    start
}

case "$1" in 
    "start")
        start
        ;;
    "stop")
        stop
        ;;
    "restart")
        restart
        ;;
    *)
        echo "$0 start|stop|restart"
esac
```

```sh
sudo chmod u+x /usr/bin/proxy
```

## step 9.
