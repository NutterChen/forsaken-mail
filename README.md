## 搭建教程

### 添加解析
如果你想邮件地址格式都为*@xx.com的形式，则为xx.com设置MX记录，需要添加以下2条解析记录。
```
#MX记录
xx.com MX 10 mx.xx.com
#A记录 
mx.xx.com A 服务器IP
```
### 安装邮件程序
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
### 配置443端口
如果你不习惯使用http//mx.xx.com:3000，或者想使用Https域名访问主界面，那我们可以使用Caddy反代。

这里所使用的域名只能是上面设置MX记录的xx.com，并提前将域名A记录解析到服务器IP。
```
#安装caddy
wget -N --no-check-certificate https://www.moerats.com/usr/shell/Caddy/caddy_install.sh && chmod +x caddy_install.sh && bash caddy_install.sh install http.filemanager

#添加反代（全部内容是一个整体，请修改2个域名后一起复制到SSH运行）
echo "xx.com {
 gzip
 tls admin@e-mail.dog
 proxy / mx.xx.com:3000
}" > /usr/local/caddy/Caddyfile

#启动caddy
/etc/init.d/caddy start

```
