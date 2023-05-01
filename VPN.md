# SS

鉴于Shadowsocks Python版本的维护情况，在较新版本的`libcrypto`库中，`EVP_CIPHER_CTX_cleanup`已被弃用，取而代之的是`EVP_CIPHER_CTX_reset`。建议使用Shadowsocks的其他实现版本，例如Shadowsocks-libev或Shadowsocks-Rust，它们具有更好的兼容性和性能。

以下是在Ubuntu上安装Shadowsocks-libev的方法：

## 添加Shadowsocks-libev的官方仓库

```shell
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev
sudo apt-get update
```



## 安装Shadowsocks-libev

```shell
sudo apt-get install shadowsocks-libev
```



## 创建配置文件

```shell
sudo nano /etc/shadowsocks-libev/config.json
```

将以下内容粘贴到配置文件中（请根据您的实际需求更改端口、密码和加密方法）：

```js
jsonCopy code{
    "server": "0.0.0.0",
    "server_port": 8388,
    "password": "your_password",
    "method": "aes-256-gcm"
}
```



## 防火墙开放端口

```shell
sudo ufw allow 8388/tcp                                                                 
sudo ufw allow 8388/udp
```



## 启动Shadowsocks-libev服务

```shell
sudo systemctl enable shadowsocks-libev
sudo systemctl start shadowsocks-libev
```





# v2ray

## 快速安装

```bash
bash <(curl -s -L  https://git.io/v2ray.sh )
```

防火墙记得开放端口。

客户端如果使用的是Clash，可以利用下面的工具将vmess生成订阅链接

https://sub-web.wcc.best/



