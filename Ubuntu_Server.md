# A) Ubuntu服务器部署

## 1. 安装Web服务软件

需要在你的Ubuntu服务器上安装一个Web服务器软件，在这里使用Apache。

```shell
sudo apt update 
sudo apt install apache2
```



## 2. 配置防火墙 

如果你的Ubuntu服务器上启用了防火墙，你需要配置防火墙以允许HTTP和HTTPS流量。你可以使用以下命令打开HTTP和HTTPS端口：

```shell
sudo ufw allow http
sudo ufw allow https
```



## 3. 创建网站目录 

你需要在你的Ubuntu服务器上创建一个网站目录，用于存放你的index.html文件和其他网站文件。

默认情况下，Apache的网站目录是/var/www/html，你可以使用以下命令创建该目录：

```shell
sudo mkdir -p /var/www/html
```

将index.html文件放入网站目录 将你的index.html文件复制到/var/www/html目录中，这样Apache就能够读取该文件并将其提供给访问者。



## 4. 配置Apache 

默认情况下，Apache会自动识别/var/www/html目录中的index.html文件作为默认文件，但是如果你想使用其他文件名作为默认文件，你可以编辑Apache的配置文件。使用以下命令打开默认的Apache配置文件：

```shell
sudo nano /etc/apache2/sites-available/000-default.conf
```

在该文件中，你可以将"DirectoryIndex"指令设置为你想要的默认文件名，比如：

```shell
DirectoryIndex myindex.html
```



## 5. 重启Apache 

使用以下命令重启Apache服务器以使配置生效：

```shell
Copy code
sudo systemctl restart apache2
```



# B) SSL证书安装

## 1. 安装Certbot客户端

```shell
sudo apt-get update
sudo apt-get install certbot python3-certbot-apache
```



## 2. 启用Apache SSL模块。您可以使用以下命令启用：

```shell
sudo a2enmod ssl
```



## 3. 重新启动Apache服务器：

```shell
sudo systemctl restart apache2
```



## 4. 使用Certbot生成证书并安装：

```shell
sudo certbot --apache
```

1. 按照提示输入您的邮箱地址和同意条款等信息。然后，Certbot将自动检测您的Apache虚拟主机配置并为您生成证书。
2. 在证书生成过程中，您将被提示选择是否将HTTP请求自动重定向到HTTPS。选择“是”以强制使用HTTPS加密。
3. 证书安装完成后，重新启动Apache服务器：

```shell
sudo systemctl restart apache2
```

现在您的网站应该已经启用了免费的SSL证书。



# C) 对新的网站启用SSL证书

## 1. 使用Certbot重新颁发证书。在执行以下命令时，将您的新域名替换为旧域名：

```bash
sudo certbot certonly --apache --domain old_domain.com -d www.old_domain.com
```

在提示输入您的邮箱地址和同意条款等信息后，Certbot将自动检测您的Apache虚拟主机配置并生成新的证书。



## 2.更新您的Apache虚拟主机配置以使用新证书。

打开您的Apache虚拟主机配置文件（通常位于`/etc/apache2/sites-available/`目录中），并将以下行中的旧域名替换为新域名：

```bash
SSLCertificateFile /etc/letsencrypt/live/old_domain.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/old_domain.com/privkey.pem
```

将上述行中的 `old_domain.com` 替换为您的新域名。



## 3. 重新启动Apache服务器：

```bash
sudo systemctl restart apache2
```

完成后，您的网站将使用更新后的证书进行加密连接。



# D) 完全卸载 Apache2 和 certbot

## 1. 停止 Apache2 服务：

```shell
sudo systemctl stop apache2
```



## 2. 卸载 Apache2 和所有依赖项：

```shell
sudo apt-get remove --purge apache2 apache2-utils
sudo apt-get autoremove
```



## 3. 删除 Apache2 配置文件和目录：

```shell
sudo rm -rf /etc/apache2
```



## 4. 停止 Apache 服务：

```shell
sudo systemctl stop apache2
```



## 5. 卸载 Certbot 客户端：

```shell
sudo apt-get remove certbot
```



## 6. 删除与 Certbot 相关的所有配置文件和目录：

```shell
sudo rm -rf /etc/letsencrypt
sudo rm -rf /var/lib/letsencrypt
```



## 7. 删除与 Certbot 相关的所有日志文件：

```shell
sudo rm -rf /var/log/letsencrypt
```



# E) 在服务器上安装ssh秘钥

## 1. 打开终端或命令提示符窗口并登录到服务器。

您可以使用 SSH 协议或其他远程访问协议登录到服务器。例如，如果您使用的是 Linux 操作系统，可以使用以下命令登录到服务器：

```shell
ssh username@server_address
```

其中 `username` 是您在服务器上的用户名，`server_address` 是服务器的 IP 地址或主机名。



## 2. 生成 SSH 密钥

```shell
ssh-keygen
```

运行该命令后，您将看到以下提示：

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa):
```

您可以按 Enter 键接受默认值或输入自己的文件名和路径。



## 3. 为私钥设置密码

您将被要求输入一个口令短语。这是为了保护您的私钥不被未经授权的人使用。如果您不想为私钥设置密码，可以直接按 Enter 键跳过此步骤。

接下来，系统会生成公钥和私钥。默认情况下，这些文件将保存在 `~/.ssh/` 目录下。公钥文件的名称是 `id_rsa.pub`，私钥文件的名称是 `id_rsa`。



## 4. 将公钥文件复制到需要使用 SSH 访问的服务器上

您可以使用以下命令将公钥复制到远程服务器：

```shell
ssh-copy-id username@server_address
```

其中 `username` 是您在服务器上的用户名，`server_address` 是服务器的 IP 地址或主机名。

如果 `ssh-copy-id` 命令不可用，则可以手动将公钥复制到远程服务器的 `~/.ssh/authorized_keys` 文件中。您可以使用以下命令将公钥复制到远程服务器：

```shell
cat ~/.ssh/id_rsa.pub | ssh username@server_address "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```

这将在远程服务器上创建 `.ssh` 目录（如果不存在），并将您的公钥追加到 `authorized_keys` 文件中。

现在您已经成功生成了 SSH 密钥，并将公钥复制到远程服务器上。下一次您使用 SSH 登录到该服务器时，您将不再需要输入密码，而是可以直接使用密钥登录。