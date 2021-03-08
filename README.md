### docker启动shadowsocks客户端

```javascript
docker run -it --restart=always --name ss -d -p 8118:8118 \
    -p 1080:1080 -e SS_SERVER="530835.s-hk-2.baacloud1.com" \
    -e SS_SERVER_PORT=38843 -e SS_SERVER_PASSWD='mtUNEyoBMB' \
    -e ENCRYPT_METHOD='aes-256-cfb' -e SS_LOCAL_PORT=1080 \
    964973791/ss:1.0.0
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

