# Linux配置

## 1. 文件系统结构

### 1.1 Linux文件系统层次结构标准（FHS）

主要目录及其用途：

- `/`：根目录
- `/home`：用户主目录
- `/etc`：系统配置文件
- `/var`：变动文件
- `/tmp`：临时文件
- `/usr`：用户程序
- `/boot`：启动文件
- `/dev`：设备文件
- `/proc`：进程信息

### 1.2 挂载点概念

- 查看挂载点：
  ```bash
  mount
  ```
- 挂载设备：
  ```bash
  sudo mount /dev/sdb1 /mnt/usb
  ```

## 2. 用户和权限管理

### 2.1 用户和组

- 创建用户：
  ```bash
  sudo adduser username
  ```
- 创建组：
  ```bash
  sudo addgroup groupname
  ```
- 将用户添加到组：
  ```bash
  sudo usermod -aG groupname username
  ```

### 2.2 文件权限

文件权限表示：`rwxrwxrwx`
- r：读
- w：写
- x：执行

使用chmod修改权限：
```bash
chmod 755 filename  # rwxr-xr-x
chmod u+x filename  # 给所有者添加执行权限
```

使用chown更改所有者：
```bash
sudo chown user:group filename
```

### 2.3 sudo和su

- sudo：以root权限执行命令
- su：切换用户

```bash
sudo command
su - username
```

## 3. 进程管理

### 3.1 查看进程

- 使用ps命令：
  ```bash
  ps aux
  ```
- 使用top命令（动态更新）：
  ```bash
  top
  ```

### 3.2 进程控制

- 结束进程：
  ```bash
  kill PID
  pkill process_name
  ```
- 后台运行：
  ```bash
  command &
  ```
- 将前台任务放到后台：
  `Ctrl + Z`，然后输入 `bg`

## 4. 网络配置

### 4.1 IP地址配置

- 查看网络接口：
  ```bash
  ip addr show
  ```
- 配置IP地址：
  ```bash
  sudo ip addr add 192.168.1.100/24 dev eth0
  ```

### 4.2 网络连接测试

- Ping：
  ```bash
  ping www.example.com
  ```
- Traceroute：
  ```bash
  traceroute www.example.com
  ```

### 4.3 SSH远程连接

- 连接到远程服务器：
  ```bash
  ssh username@remote_host
  ```
- 生成SSH密钥：
  ```bash
  ssh-keygen -t rsa -b 4096
  ```