# 实践课：Docker容器技术

## 1. Docker安装与配置

首先，需要在系统上安装Docker。以Ubuntu系统为例：

```bash
# 更新包索引
sudo apt-get update

# 安装必要的包
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# 添加Docker的官方GPG密钥
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# 设置稳定版仓库
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# 更新包索引
sudo apt-get update

# 安装最新版本的Docker CE
sudo apt-get install docker-ce

# 验证Docker是否安装成功
docker --version
```

## 2. Docker基本操作

### 2.1 镜像操作

```bash
# 搜索镜像
docker search ubuntu

# 拉取镜像
docker pull ubuntu:latest

# 列出本地镜像
docker images

# 删除镜像
docker rmi ubuntu:latest
```

### 2.2 容器操作

```bash
# 运行容器
docker run -it ubuntu /bin/bash

# 列出正在运行的容器
docker ps

# 列出所有容器（包括停止的）
docker ps -a

# 停止容器
docker stop <container_id>

# 启动已停止的容器
docker start <container_id>

# 删除容器
docker rm <container_id>
```

## 3. Dockerfile实践

创建一个简单的Python Web应用并将其容器化。

1. 创建应用文件 `app.py`：

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, Docker!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

2. 创建 `requirements.txt`：

```
Flask==2.0.1
```

3. 创建 `Dockerfile`：

```dockerfile
# 使用官方Python运行时作为父镜像
FROM python:3.9-slim-buster

# 设置工作目录
WORKDIR /app

# 将当前目录内容复制到容器的/app中
COPY . /app

# 安装需求包
RUN pip install --no-cache-dir -r requirements.txt

# 声明运行时端口
EXPOSE 5000

# 运行app.py
CMD ["python", "app.py"]
```

4. 构建镜像：

```bash
docker build -t my-flask-app .
```

5. 运行容器：

```bash
docker run -p 5000:5000 my-flask-app
```

现在可以在浏览器中访问 `http://localhost:5000` 来查看应用。

## 4. Docker Compose实践

创建一个包含Web应用和数据库的多容器应用。

1. 修改 `app.py`：

```python
import os
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    return 'Hello Docker! I have been seen {} times.'.format(redis.get('hits'))

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

2. 更新 `requirements.txt`：

```
Flask==2.0.1
redis==3.5.3
```

3. 创建 `docker-compose.yml`：

```yaml
version: '3'
services:
  web:
    build: .
    ports:
     - "5000:5000"
  redis:
    image: "redis:alpine"
```

4. 启动服务：

```bash
docker-compose up
```

现在，当访问 `http://localhost:5000` 时，每次刷新页面，计数器都会增加。

## 5. Docker网络

Docker提供了几种网络驱动程序：bridge、host、overlay、macvlan、none。

```bash
# 创建自定义网络
docker network create my-net

# 列出网络
docker network ls

# 在自定义网络中运行容器
docker run -d --network my-net --name my-nginx nginx

# 检查网络
docker network inspect my-net
```

## 6. Docker数据管理

### 6.1 数据卷

```bash
# 创建数据卷
docker volume create my-vol

# 使用数据卷运行容器
docker run -d -v my-vol:/app nginx
```

### 6.2 绑定挂载

```bash
# 使用绑定挂载运行容器
docker run -d -v /host/path:/container/path nginx
```

## 扩展资源

- [Docker官方文档](https://docs.docker.com/)
- [Docker Curriculum](https://docker-curriculum.com/)
- [Play with Docker Classroom](https://training.play-with-docker.com/)

## 总结

这个Docker实践课程涵盖了从基本安装到高级特性的广泛内容。通过hands-on练习，学习者可以掌握Docker的核心概念和操作，包括镜像和容器管理、Dockerfile的编写、多容器应用的编排、网络配置以及数据管理。这些技能为在实际开发和部署中使用Docker奠定了坚实的基础。随着云原生技术的不断发展，掌握Docker已成为现代开发者的必备技能。