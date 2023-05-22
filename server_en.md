# A) Ubuntu Server Deployment

## 1. Install Web Server Software

You need to install a web server software on your Ubuntu server, and here we will use Apache.

```shell
sudo apt update 
sudo apt install apache2
```

## 2. Configure Firewall

If the firewall is enabled on your Ubuntu server, you need to configure it to allow HTTP and HTTPS traffic. You can use the following commands to open the HTTP and HTTPS ports:

```shell
sudo ufw allow http
sudo ufw allow https
```

## 3. Create Website Directory

You need to create a website directory on your Ubuntu server to store your index.html file and other website files.

By default, the website directory for Apache is /var/www/html, and you can create it using the following command:

```shell
sudo mkdir -p /var/www/html
```

Put the index.html File into the Website Directory

Copy your index.html file to the /var/www/html directory, so that Apache can read the file and serve it to visitors.

## 4. Configure Apache

By default, Apache automatically recognizes the index.html file in the /var/www/html directory as the default file. However, if you want to use a different filename as the default file, you can edit Apache's configuration file. Use the following command to open the default Apache configuration file:

```shell
sudo nano /etc/apache2/sites-available/000-default.conf
```

In this file, you can set the "DirectoryIndex" directive to the default file name you want, such as:

```
DirectoryIndex myindex.html
```

## 5. Restart Apache

Use the following command to restart the Apache server to make the configuration take effect:

```shell
sudo systemctl restart apache2
```



# B) SSL Certificate Installation

## 1. Install Certbot Client

```shell
sudo apt-get update
sudo apt-get install certbot python3-certbot-apache
```

## 2. Enable Apache SSL Module

```shell
sudo a2enmod ssl
```

## 3. Restart Apache Server

```shell
sudo systemctl restart apache2
```

## 4. Generate and Install Certificate Using Certbot

```shell
sudo certbot --apache
```

1. Follow the prompts to enter your email address and agree to the terms, etc. Then, Certbot will automatically detect your Apache virtual host configuration and generate a certificate for you.
2. During the certificate generation process, you will be prompted to choose whether to automatically redirect HTTP requests to HTTPS. Choose "yes" to enforce HTTPS encryption.
3. After the certificate installation is complete, restart the Apache server:

```shell
sudo systemctl restart apache2
```

Now your website should have a free SSL certificate enabled.





# C) Enable SSL Certificate for a New Website

## 1. Reissue the Certificate with Certbot

When executing the following command, replace your new domain name with the old one:

```shell
sudo certbot certonly --apache --domain old_domain.com -d www.old_domain.com
```

After entering your email address and agreeing to the terms, etc., Certbot will automatically detect your Apache virtual host configuration and generate a new certificate.

## 2. Update your Apache virtual host configuration to use the new certificate

Open your Apache virtual host configuration file (usually located in the `/etc/apache2/sites-available/` directory) and replace the old domain name with the new one in the following lines:

```
SSLCertificateFile /etc/letsencrypt/live/old_domain.com/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/old_domain.com/privkey.pem
```

Replace `old_domain.com` with your new domain name in the above lines.

## 3. Restart Apache Server

```shell
sudo systemctl restart apache2
```

After completion, your website will be encrypted with the updated certificate.



# D) Completely Uninstall Apache2 and Certbot

## 1. Stop Apache2 Service

```shell
sudo systemctl stop apache2
```

## 2. Uninstall Apache2 and All Dependencies

```shell
sudo apt-get remove --purge apache2 apache2-utils
sudo apt-get autoremove
```

## 3. Delete Apache2 Configuration Files and Directories

```shell
sudo rm -rf /etc/apache2
```

## 4. Stop Apache Service

```shell
sudo systemctl stop apache2
```

## 5. Uninstall Certbot Client

```shell
sudo apt-get remove certbot
```

## 6. Delete All Configuration Files and Directories Related to Certbot

```shell
sudo rm -rf /etc/letsencrypt
sudo rm -rf /var/lib/letsencrypt
```

## 7. Delete All Log Files Related to Certbot

```shell
sudo rm -rf /var/log/letsencrypt
```



# E) Install SSH Key on Server

## 1. Open a Terminal or Command Prompt window and log in to the server.

You can log in to the server using SSH protocol or other remote access protocols. For example, if you are using a Linux operating system, you can use the following command to log in to the server:

```shell
ssh username@server_address
```

Where `username` is your username on the server and `server_address` is the IP address or hostname of the server.

## 2. Generate an SSH Key

```shell
ssh-keygen
```

After running this command, you will see the following prompt:

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/home/username/.ssh/id_rsa):
```

You can press Enter to accept the default value or enter your own filename and path.

## 3. Set a Passphrase for the Private Key

You will be prompted to enter a passphrase. This is to protect your private key from unauthorized use. If you do not want to set a passphrase for the private key, you can simply press Enter to skip this step.

The system will then generate the public and private keys. By default, these files are saved in the `~/.ssh/` directory. The name of the public key file is `id_rsa.pub`, and the name of the private key file is `id_rsa`.

## 4. Copy the Public Key File to the Server for SSH Access

You can use the following command to copy the public key to the remote server