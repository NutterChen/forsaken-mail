Forsaken-Mail
==============
A self-hosted disposable mail service.

[Online Demo](http://disposable.dhc-app.com)

### Installation

#### Setting up your DNS correctly

In order to receive emails, your smtp server address should be made available somewhere. Two records should be added to your DNS records. Let us pretend that we want to receive emails at ```*@subdomain.domain.com```:
* First an MX record: ```subdomain.domain.com MX 10 mxsubdomain.domain.com```. This means that the mail server for addresses like ```*@subdomain.domain.com``` will be ```mxsubdomain.domain.com```.
* Then an A record: ```mxsubdomain.domain.com A the.ip.address.of.your.mailin.server```. This tells at which ip address the mail server can be found.

You can fire up Mailin (see next section) and use an [smtp server tester](http://mxtoolbox.com/diagnostic.aspx) to verify that everything is correct.

#### 搭建教程
```
#安装git
yum install git -y
 
#安装nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
 
#安装nodejs和 npm
nvm install 6.10.0
 
#查看nodejs版本是否正确，显示 6.10.0
node -v
 
#下载项目源码
git clone https://github.com/malaohu/forsaken-mail.git
cd forsaken-mail
 
#安装项目需要的库
npm install
 
#安装pm2工具
npm install -g pm2
 
#禁用postfix
postfix stop
chkconfig --level 2345 postfix off
 
#启动项目
pm2 start bin/www
#设置开机启动
pm2 startup
pm2 save
```
