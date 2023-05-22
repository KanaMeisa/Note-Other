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



## 2. 启用Apache SSL模块

```shell
sudo a2enmod ssl
```



## 3. 重新启动Apache服务器

```shell
sudo systemctl restart apache2
```



## 4. 使用Certbot生成证书并安装

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

## 1. 使用Certbot重新颁发证书

在执行以下命令时，将您的新域名替换为旧域名：

```bash
sudo certbot certonly --apache --domain old_domain.com -d www.old_domain.com
```

在提示输入您的邮箱地址和同意条款等信息后，Certbot将自动检测您的Apache虚拟主机配置并生成新的证书。



## 2.更新您的Apache虚拟主机配置以使用新证书

打开您的Apache虚拟主机配置文件（通常位于`/etc/apache2/sites-available/`目录中），并将以下行中的旧域名替换为新域名：

```bash
SSLCertificateFile /etc/letsencrypt/live/old_domain.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/old_domain.com/privkey.pem
```

将上述行中的 `old_domain.com` 替换为您的新域名。



## 3. 重新启动Apache服务器

```bash
sudo systemctl restart apache2
```

完成后，您的网站将使用更新后的证书进行加密连接。



# D) 完全卸载 Apache2 和 certbot

## 1. 停止 Apache2 服务

```shell
sudo systemctl stop apache2
```



## 2. 卸载 Apache2 和所有依赖项

```shell
sudo apt-get remove --purge apache2 apache2-utils
sudo apt-get autoremove
```



## 3. 删除 Apache2 配置文件和目录

```shell
sudo rm -rf /etc/apache2
```



## 4. 停止 Apache 服务

```shell
sudo systemctl stop apache2
```



## 5. 卸载 Certbot 客户端

```shell
sudo apt-get remove certbot
```



## 6. 删除与 Certbot 相关的所有配置文件和目录

```shell
sudo rm -rf /etc/letsencrypt
sudo rm -rf /var/lib/letsencrypt
```



## 7. 删除与 Certbot 相关的所有日志文件

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





# F) 后台运行程序

在Ubuntu服务器终端中，您可以使用以下命令在后台运行程序

```shell
nohup command &
```

其中，`command`是您要在后台运行的程序或命令。`nohup`命令可确保即使关闭终端或断开与服务器的连接，程序仍然可以继续运行。`&`符号将程序置于后台运行。



例如，如果您想要在后台运行一个名为`myprogram.py`的Python程序，您可以使用以下命令：

```shell
nohup python myprogram.py &
```

这将启动`myprogram.py`程序并将其置于后台运行，即使您关闭终端或断开与服务器的连接，该程序仍将继续运行。



要停止在后台运行的程序，可以使用以下命令：

查找程序的进程ID（PID）：

```shell
ps aux | grep command
```

其中，`command`是您要停止的程序的名称或命令。该命令将列出所有包含该命令名称的进程，以及它们的PID和其他信息。



使用`kill`命令停止该进程：

```shell
kill PID
```

其中，`PID`是您要停止的进程的PID。



例如，如果您要停止一个名为`myprogram.py`的Python程序，可以按以下方式操作：

查找该程序的进程ID：

```shell
ps aux | grep myprogram.py
```

该命令将列出包含`myprogram.py`命令的所有进程及其PID。



找到您想要停止的进程的PID，并使用`kill`命令停止它：

```sh
kill PID
```

其中，`PID`是您要停止的进程的PID。例如，如果该程序的PID为`1234`，则使用以下命令停止它：

```sh
kill 1234
```





# G) Apache同时服务两个域名



要让Apache服务器同时为两个域名提供服务，您需要使用名为“虚拟主机”的功能。虚拟主机允许您在同一台服务器上托管多个独立的网站。以下是在Apache服务器上设置两个虚拟主机的方法：

在服务器上创建两个不同的目录，用于存放两个域名的网站文件。例如：

```
/var/www/website1
/var/www/website2
```

将您的网站文件（HTML、CSS、JavaScript等）放入这些目录中。

转到Apache的虚拟主机配置文件。通常，配置文件位于以下位置之一：

- `/etc/httpd/conf.d/vhost.conf`（CentOS）
- `/etc/apache2/sites-available/000-default.conf`（Ubuntu/Debian）

如果在这些位置找不到配置文件，请查找Apache主配置文件（例如`httpd.conf`或`apache2.conf`），然后找到包含虚拟主机配置的部分。

在虚拟主机配置文件中，为两个域名创建两个名为`VirtualHost`的条目。例如：

```apache
<VirtualHost *:80>
    ServerName domain1.com
    ServerAlias www.domain1.com
    DocumentRoot /var/www/website1

    <Directory /var/www/website1>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/domain1_error.log
    CustomLog ${APACHE_LOG_DIR}/domain1_access.log combined
</VirtualHost>

<VirtualHost *:80>
    ServerName domain2.com
    ServerAlias www.domain2.com
    DocumentRoot /var/www/website2

    <Directory /var/www/website2>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/domain2_error.log
    CustomLog ${APACHE_LOG_DIR}/domain2_access.log combined
</VirtualHost>
```



在此示例中，将`domain1.com`和`domain2.com`替换为您的域名，同时确保`DocumentRoot`指向您在步骤2中创建的目录。

保存配置文件并退出编辑器。

重启Apache服务器，以便更改生效。在大多数系统上，您可以使用以下命令之一重启Apache：

```sh
sudo systemctl restart httpd         # CentOS
sudo systemctl restart apache2       # Ubuntu/Debian
```

确保将您的域名解析为托管Apache服务器的IP地址。这需要在您的域名注册商的DNS设置中进行操作。

现在，当您访问这两个域名时，Apache服务器将分别为它们提供不同的网站内容。如果您需要使用HTTPS，请确保已正确配置SSL证书，并将`VirtualHost`条目更改为监听443端口。
