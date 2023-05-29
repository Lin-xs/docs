# AntGrid on WSL2

## 环境
Windows 10 + WSL2 Ubuntu 18.04

## p2pdatabase
此部分安装用户信息管理后端,即账号密码等.仓库位于https://github.com/Lin-xs/p2pdatabase.

按照README执行完"运行方法"一节即可.默认运行`uvicorn main:app --host 0.0.0.0 --port 3000`

此时打开浏览器访问`localhost:3000/docs`,若出现FastAPI文档即为配置完成.

### 可能出现的问题:
* WSL下查看mysql运行状态请运行`sudo service mysql status`,而非`systemctl`.
* 安装完成后运行`mysql`,报错
    ```
    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
    ```
    解决方法为将`/etc/mysql/my.cnf `中的`bind-address = 127.0.0.1`改成`bind-address = localhost`.注意,如果你`my.cof`没有这一项,但文件中包含了其他目录,比如:
    ```
    !includedir /etc/mysql/conf.d/
    !includedir /etc/mysql/mysql.conf.d/
    ```
    则检索这两个目录下的配置文件中对于bind的设置并替换.例如:
    ```
    $ cat /etc/mysql/mysql.conf.d/mysqld.cnf  | grep address
    bind-address            = 127.0.0.1
    ```
    那么将此处改动即可.随后重启:`sudo /etc/init.d/mysql restart`.之后再尝试运行mysql.

* `sudo apt install mysql-server`后,并没有设置root密码的部分.此时先运行`sudo mysql_secure_installation`设置root密码(默认使用123456),其余部分均回车跳过.随后`sudo mysql -u root -p`使用刚才的密码登录(此时如果不加sudo直接用root登录,可能会报错).随后运行`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';`设置你的root密码.之后应该就可以正常`mysql -u root -p`了.

## NodeJS
查看node版本:
```
node -v
```
我使用的版本为v16.20.0.若未安装nodejs,可采用nvm方式安装:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
source .bashrc
nvm install v16.20.0
```

此部分仓库位于https://github.com/xwhzz/demo.
完成README中的"快速开始"部分即可.

