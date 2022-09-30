### 在线安装脚本或官网

[Ubuntu Docker 安装 | 菜鸟教程 (runoob.com)](https://www.runoob.com/docker/ubuntu-docker-install.html)

https://github.com/wsargent/docker-cheat-sheet/tree/master/zh-cn

https://www.runoob.com/docker/docker-run-command.html

### 配置源

```Bash
# 配置源 | 创建或修改/etc/docker/daemon.json
{
 "registry-mirrors" : [
   "https://mirror.ccs.tencentyun.com",
   "http://registry.docker-cn.com",
   "http://docker.mirrors.ustc.edu.cn",
   "http://hub-mirror.c.163.com"
 ],
 "insecure-registries" : [
   "registry.docker-cn.com",
   "docker.mirrors.ustc.edu.cn"
 ],
 "debug" : true,
 "experimental" : true
}
```

### 启动docker

```Bash
# systemctl方式
systemctl restart docker.service
```

### docker开机自启

```Bash
# docker开机启动
systemctl enable docker
```

### 容器自启

```Bash
docker run 指令中加入 --restart=always 
# 容器已创建
docker update --restart=always 容器名
```

### 关启所有容器

```Bash
# docker中启动所有的容器命令
docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
# docker中关闭所有的容器命令
docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
```

### 查看容器挂载目录

```Bash
docker inspect container_id | grep Mounts -A 20
```

### 容器内时区异常

```Bash
# 进入容器
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo "Asia/Shanghai" > /etc/timezone
```



### 常用容器创建

#### MySQL 8.0

安装：https://juejin.cn/search?utm_source=infinitynewtab.com&query=docker mysql8

官方：https://dev.mysql.com/doc/mysql-installation-excerpt/8.0/en/linux-installation.html

```Plaintext
docker run -p 77:3306 --restart=always --name mysql8.0 -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0
备注：
-p 将本地主机的端口映射到docker容器端口（因为本机的3306端口已被其它版本占用，所以使用3307）
--name 容器名称命名
-e 配置信息，配置root密码
-d 镜像名称

# 8.0改变了鉴权方式导致navicat可能无法连接
-------------------------------- 进入容器-------------------------------------------
mysql -uroot -p
use mysql
alter user 'root'@'%' identified with mysql_native_password by 'root';
select host,user,plugin from user;

改密码
mysqladmin -u root -p password 新密码
```



#### Tomcat

```Plaintext
docker run -p 5001:8080 -v /root/project_home/docker/tsp_web:/usr/local/tomcat/webapps --name tsp_web -d tomcat:8.5

# docker tomcat时区异常 在容器内
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo "Asia/Shanghai" > /etc/timezone
```



#### Nginx

```Plaintext
docker run --name nginx -p 96:80 -v /xslt/data:/data -d nginx
```
