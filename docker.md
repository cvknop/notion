[TOC]



## 快捷键

> idea

### idea

| key        | value                |
| ---------- | -------------------- |
| ctrl+alt+l | 调整格式             |
| ctrl + p   | 方法参数提示         |
| ctrl + b   | 查看方法在本地的实现 |
| ctrl+alt+b | 查看所有实现类       |
| ctrl + q   | 提示信息             |
| alt + 7    | 查看某类所有方法     |

https://zhuanlan.zhihu.com/p/61690346

## Spring

### 注解

```Java
// swagger
@ApiImplicitParams({@ApiImplicitParam(name = "id", value = "主键", required = true)})

// date to front-end
from jackson
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone="GMT+8")
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone="GMT+8",shape=JsonFormat.Shape.STRING)
from fastjson
@JSONField(format = "yyyy-MM-dd HH:mm:ss")

// date to back-end
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone="GMT+8")

-----------------parameter validation-----------------javax.validation.constraints.*

diff @Validated @Valid  $link:[csdn](<https://blog.csdn.net/qq_27680317/article/details/79970590>)  <https://juejin.cn/post/6844903677119954958>

// $link:[史上最全的正则表达式-匹配中英文、字母和数字](<https://blog.csdn.net/qq_28633249/article/details/77686976>)
@Pattern(regexp = "^[\\\\u4e00-\\\\u9fa5a-zA-Z0-9]+$", message = "个人简介仅支持中英文数字")
```

### 测试

```Java
// 需要有启动类
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
// 注意导包是下面这个
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class ProxyTest {

    /**
     * public
     */
    @Test
    public void testStaticProxy() {
   
    }
}
 <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.1</version>
    <scope>test</scope>
</dependency>
```

### 构造器注入

```Java
private final DbService dbService;
    
    public DbController(@Qualifier("dbProxy") DbService dbService) {
        this.dbService = dbService;
    }
```

## 常用命令

### 查杀端口

```Bash
# centos查杀端口
netstat -lnp|grep 8000
lsof -i:{端口号}
kill -9 {PID}

# 根据端口查杀进程
kill -9 $(lsof -i:8848 |awk '{print $2}' | tail -n 2)
# win查杀端口
netstat -nao | findstr 8000
taskkill -pid 3692 -f
```

### 开放端口

```Bash
使用sudo ufw enable开启防火墙
# 开启端口
firewall-cmd --zone=public --add-port=9001/tcp --permanent
systemctl restart firewalld.service
```

## Ubantu虚拟机相关

### 开启root登录和ssh

```Bash
vim /etc/hostname

# ubantu21 开启root登录

sudo passwd

vim /etc/ssh/sshd_config 
# 注释这行
PermitRootLogin prohibit-password
# 在这行底下添加
PermitRootLogin yes（在第29行添加） 
# 重启 ssh 服务即可
service sshd restart

# 安装ssh-server
# 切换阿里源 设置中----
sudo apt-get update
sudo apt-get install openssh-server

# 检测是否已启动
sudo ps -e|grep ssh
--sshd

sudo service ssh start

# 开启密钥登陆 /root/.ssh
cd .ssh 
ssh-copy-id -i rsa2.pub user@host
```

### 解决中文乱码

```Bash
检查是否已经安装了中文包支持

sudo dpkg -l | grep language-pack-zh-hans
安装
如果没有安装，就安装，已经安装了就直接跳到下一步，原来的language-pack-zh已经更新为language-pack-zh-hans，所以这一步改了一下

sudo apt-get install language-pack-zh-hans
配置语言环境变量
1.打开/etc/environment

sudo vim /etc/environment
把下面代码添加进environment文件
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
2.打开/var/lib/locales/supported.d/local，文件不存在就创建文件

sudo vim /var/lib/locales/supported.d/local
添加zh_CN.GB2312字符集
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
zh_CN.GBK GBK
zh_CN GB2312
3.保存文件，执行以下命令

sudo locale-gen
设置系统默认语言
打开/etc/default/locale

sudo vim /etc/default/locale
修改为：

LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en" 
sudo reboot
然后重新开启终端就搞定了
```

### 修改host

```undefined
sudo vim /etc/hosts
#e.g 172.24.110.2 gitlab.openjawtech.com
sudo /etc/init.d/dns-clean start
sudo /etc/init.d/networking restart
```

## 环境

> 非安装包安装

### JDK

https://juejin.cn/post/7031523681058684942 

```Bash
# 查看安装位置
sudo update-alternatives --config java
# e.g: /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
# dir: /usr/lib/jvm/java-8-openjdk-amd64

# set jdk
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

source /etc/profile
```

### Maven

```Bash
sudo apt install maven
sudo chmod 777 /usr/share/maven/conf/settings.xml
sudo vim /usr/share/maven/conf/settings.xml

sudo vim /etc/profile
export MAVEN_HOME=/usr/share/maven
export PATH=${MAVEN_HOME}/bin:${PATH}

source /etc/profile
```

### Git

```Bash
git config --global user.name "cvknop"
git config --global user.email "hj@travelsky.com.cn"
git config --global core.autocrlf false #关闭自动换行的设置
git config --global core.editor vim #设置git操作文件时使用的编辑软件
ssh-keygen -t rsa -C 'hj@travelsky.com.cn'

sudo vim /etc/hosts
```

