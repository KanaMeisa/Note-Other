# Shadowsocks

Due to the maintenance status of the Shadowsocks Python version, `EVP_CIPHER_CTX_cleanup` has been deprecated in newer versions of the `libcrypto` library and replaced with `EVP_CIPHER_CTX_reset`. It is recommended to use other implementations of Shadowsocks, such as Shadowsocks-libev or Shadowsocks-Rust, which have better compatibility and performance.

Here are the steps to install Shadowsocks-libev on Ubuntu:

## Add Shadowsocks-libev Official Repository

```
shellCopy codesudo apt-get install software-properties-common
sudo add-apt-repository ppa:max-c-lv/shadowsocks-libev
sudo apt-get update
```

## Install Shadowsocks-libev

```
shellCopy code
sudo apt-get install shadowsocks-libev
```

## Create Configuration File

```
shellCopy code
sudo nano /etc/shadowsocks-libev/config.json
```

Paste the following content into the configuration file (change the port, password, and encryption method according to your needs):

```
jsCopy codejsonCopy code{
    "server": "0.0.0.0",
    "server_port": 8388,
    "password": "your_password",
    "method": "aes-256-gcm"
}
```

## Open Firewall Port

```
shellCopy codesudo ufw allow 8388/tcp                                                                 
sudo ufw allow 8388/udp
```

## Start Shadowsocks-libev Service

```
shellCopy codesudo systemctl enable shadowsocks-libev
sudo systemctl start shadowsocks-libev
```



# v2ray

## Quick Installation

```
bashCopy code
bash <(curl -s -L  https://git.io/v2ray.sh )
```

Remember to open the port on the firewall.

If you use Clash as the client, you can use the following tool to generate a subscription link for vmess:

https://sub-web.wcc.best/