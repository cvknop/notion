[TOC]

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

