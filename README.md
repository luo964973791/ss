### docker启动shadowsocks客户端

```javascript
docker run -it -d -p 1080:1080 --restart=always --name ss -e SERVER='530835.s-hk-2.baidu.com' -e PORT=38843 -e LISTEN=1080 -e METHOD='aes-256-cfb' -e PASSWD='mtUNEyoBMB' 964973791/ss:1.0.0
```

### 验证shadowsocks代理是否正常.

```javascript
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
{
"origin": "x.x.x.x" #你的Shadowsock服务器IP
}
```

### 安装Privoxy

```javascript
docker run -d --name=privoxy --restart=always --net=host -p 8118:8118 964973791/privoxy:1.0.0
```

### 配置全局代理

```javascript
vi ~/.bashrc
export http_proxy=http://127.0.0.1:8118
export https_proxy=https://127.0.0.1:8118
```

### 验证

```javascript
#返回一大堆HTML 则说明正常
curl www.google.com
#有如下相似显示则说明正常
curl -I www.google.com
```

