### 安装shadowsocks客户端

```javascript
yum install -y epel-release
yum install -y python-pip libsodium
pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U
```

### 配置shadowsocks客户端

```javascript
mkdir /etc/shadowsocks
vi /etc/shadowsocks/shadowsocks.json
{
    "server":"221.228.99.199",
    "server_port":584,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"cc.ax",
    "timeout":300,
    "method":"chacha20-ietf",
    "fast_open": false,
    "workers": 1
}
```

### 配置开机自启动

```javascript
vi /etc/systemd/system/shadowsocks.service
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/local/bin/sslocal -c /etc/shadowsocks/shadowsocks.json

[Install]
WantedBy=multi-user.target


#设置开机自启动
systemctl enable shadowsocks.service
systemctl start shadowsocks.service
systemctl status shadowsocks.service
```

### 验证shadowsocks代理是否正常.

```javascript
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
{
"origin": "x.x.x.x" #你的Shadowsock服务器IP
}
```

### 安装Privoxy,注意下面两行没有被注释掉.

```javascript
yum -y install privoxy
vi /etc/privoxy/config
listen-address 127.0.0.1:8118 # 8118 是默认端口，不用改  
forward-socks5t / 127.0.0.1:1080 . #转发到本地端口，注意别忘了最后的.


systemctl enable privoxy
systemctl start privoxy
systemctl status privoxy
```

### 配置全局代理

```javascript
vi /etc/profile
export http_proxy=http://127.0.0.1:8118
export https_proxy=http://127.0.0.1:8118
```

### 验证

```javascript
#返回一大堆HTML 则说明正常
curl www.google.com
#有如下相似显示则说明正常
curl -I www.google.com
```

