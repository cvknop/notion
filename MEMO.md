[TOC]

## Linux



### 1. 注解

#### 1.1 swagger

```java
@ApiImplicitParams({@ApiImplicitParam(name = "id", value = "主键", required = true)})

```



#### 1.2 Json

```java
// date to front-end
jackson
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone="GMT+8",shape=JsonFormat.Shape.STRING)

// fastjson
@JSONField(format = "yyyy-MM-dd HH:mm:ss")

// date to back-end
@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone="GMT+8")
```



1.3 常见正则

[史上最全的正则表达式-匹配中英文、字母和数字_杨先森的博客的博客-CSDN博客_正则匹配字母](https://blog.csdn.net/qq_28633249/article/details/77686976)

```
@Pattern(regexp = "^[\\\\u4e00-\\\\u9fa5a-zA-Z0-9]+$", message = "个人简介仅支持中英文数字")
```

```java
@Validated 
@Valid 
```



### 2. Springboot 测试

要有启动类

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```

测试类模板

```java
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
```

依赖

```xml
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



### 3. 构造器注入

```java
private final DbService dbService;

// @Autowired这里可加可不加
public DbController(@Qualifier("dbProxy") DbService dbService) {this.dbService = dbService;}
```



### 4. 读配置文件

读其他yml

```java
@Component
@Data
@PropertySource(value = {"classpath:bootstrap.yml"}, factory = YamlAndPropertySourceFactory.class)
@ConfigurationProperties(prefix = "test")
public class InitConfig {

    //@Value("${test.username}")
    private String password;

    //@Value("${test.password}")
    private String userName;
}

```



```java
import org.springframework.boot.env.YamlPropertySourceLoader;
import org.springframework.core.env.PropertiesPropertySource;
import org.springframework.core.env.PropertySource;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.DefaultPropertySourceFactory;
import org.springframework.core.io.support.EncodedResource;

import java.io.IOException;
import java.util.List;
import java.util.Objects;
import java.util.Properties;

/**
 * @author jhuan
 * @version 1.0
 * @description: TODO
 * @date 2022年09月28日 22:11
 */
public class YamlAndPropertySourceFactory extends DefaultPropertySourceFactory {
    @Override
    public PropertySource<?> createPropertySource(String name, EncodedResource resource) throws IOException {
        Resource resourceResource = resource.getResource();
        if (!resourceResource.exists()) {
            return new PropertiesPropertySource(null, new Properties());
        } else if (Objects.requireNonNull(resourceResource.getFilename()).endsWith(".yml") || resourceResource.getFilename().endsWith(".yaml")) {
            List<PropertySource<?>> sources = new YamlPropertySourceLoader().load(resourceResource.getFilename(), resourceResource);
            return sources.get(0);
        }
        return super.createPropertySource(name, resource);
    }
}
```

