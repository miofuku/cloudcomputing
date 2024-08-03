# 实践课：LAMP环境搭建

## 1. 准备工作

在开始之前，确保使用的是Ubuntu 20.04或更新版本的系统。打开终端，首先更新系统包列表：

```bash
sudo apt update && sudo apt upgrade -y
```

## 2. 安装Apache Web服务器

### 2.1 安装Apache

```bash
sudo apt install apache2 -y
```

### 2.2 检查Apache状态

```bash
sudo systemctl status apache2
```

如果Apache正在运行，将看到"active (running)"的状态。

### 2.3 配置防火墙

如果使用UFW防火墙，需要允许Apache通过：

```bash
sudo ufw allow in "Apache Full"
```

### 2.4 测试Apache

在浏览器中访问`http://localhost`或`http://服务器IP地址`，应该能看到Apache2默认页面。

## 3. 安装MySQL

### 3.1 安装MySQL服务器

```bash
sudo apt install mysql-server -y
```

### 3.2 运行安全脚本

这个脚本会引导完成一些安全设置：

```bash
sudo mysql_secure_installation
```

按照提示设置root密码，删除匿名用户，禁止root远程登录，并删除测试数据库。

### 3.3 验证MySQL安装

```bash
sudo systemctl status mysql
```

## 4. 安装PHP

### 4.1 安装PHP及常用模块

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

这会安装PHP、Apache PHP模块和MySQL PHP扩展。

### 4.2 验证PHP安装

创建一个PHP信息页面：

```bash
sudo echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/phpinfo.php
```

在浏览器中访问`http://localhost/phpinfo.php`或`http://服务器IP地址/phpinfo.php`，应该能看到PHP信息页面。

### 4.3 配置PHP

编辑PHP配置文件：

```bash
sudo nano /etc/php/7.4/apache2/php.ini
```

可以根据需要调整一些常用设置，如：
- `memory_limit = 256M`
- `upload_max_filesize = 64M`
- `post_max_size = 64M`
- `max_execution_time = 300`

编辑完成后，重启Apache：

```bash
sudo systemctl restart apache2
```

## 5. 安装phpMyAdmin（可选）

phpMyAdmin是一个流行的MySQL图形管理工具。

### 5.1 安装phpMyAdmin

```bash
sudo apt install phpmyadmin -y
```

在安装过程中，选择配置为Apache2，并设置phpMyAdmin的数据库密码。

### 5.2 配置Apache以使用phpMyAdmin

创建一个配置文件：

```bash
sudo nano /etc/apache2/conf-available/phpmyadmin.conf
```

添加以下内容：

```apache
Alias /phpmyadmin /usr/share/phpmyadmin
<Directory /usr/share/phpmyadmin>
    Options SymLinksIfOwnerMatch
    DirectoryIndex index.php
    AllowOverride All
    Require all granted
</Directory>
```

启用配置并重启Apache：

```bash
sudo a2enconf phpmyadmin
sudo systemctl restart apache2
```

现在可以通过`http://localhost/phpmyadmin`或`http://服务器IP地址/phpmyadmin`访问phpMyAdmin。

## 6. 测试LAMP堆栈

创建一个测试PHP文件：

```bash
sudo nano /var/www/html/test.php
```

添加以下内容：

```php
<?php
$servername = "localhost";
$username = "root";
$password = "你的MySQL root密码";

// 创建连接
$conn = new mysqli($servername, $username, $password);

// 检查连接
if ($conn->connect_error) {
    die("连接失败: " . $conn->connect_error);
}
echo "连接成功";

// 关闭连接
$conn->close();
?>
```

在浏览器中访问`http://localhost/test.php`或`http://服务器IP地址/test.php`，如果看到"连接成功"的消息，说明LAMP堆栈已经成功搭建。

## 相关资源

- [Apache 官方文档](https://httpd.apache.org/docs/)
- [MySQL 官方文档](https://dev.mysql.com/doc/)
- [PHP 官方文档](https://www.php.net/manual/en/)
- [phpMyAdmin 文档](https://docs.phpmyadmin.net/)

## 总结

这个实践课程指导完成了LAMP（Linux、Apache、MySQL、PHP）环境的搭建。通过这个过程，可以了解到每个组件的基本安装和配置步骤。这个环境为开发动态网站和Web应用程序提供了坚实的基础。实践中遇到的各种命令和配置文件也加深了对Linux系统管理的理解。完成这个搭建过程后，就可以开始开发各种Web应用了，比如WordPress、Drupal或自定义的PHP应用。