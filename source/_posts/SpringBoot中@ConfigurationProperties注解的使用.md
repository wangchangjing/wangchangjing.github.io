---
title: SpringBoot中@ConfigurationProperties注解的使用
date: 2019-01-21 17:58:30
tags:
categories: Spring Boot
description: 
---


#### 引言
有时候我们想把配置文件的信息，在项目启动时候自动读取并绑定到实体类中，可以使用@ConfigurationProperties注解来实现。
#### 方式一：@Component+@ConfigurationProperties

通过`@Component`和`@ConfigurationProperties`两个注解，定义Spring的一个实体Bean来装载配置信息，需要使用配置信息时注入该实体Bean即可  

创建Person类  

```
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
@ConfigurationProperties(prefix = "demo.person")
public class PersonProperties {

    private String name;

    private Integer age;

    private List<String> address;

    // 省去set、get、toString方法
}
```
application.properties配置

```
server.port=8290

demo.person..name=张三
demo.person.age=18
demo.person.address[0]=北京
demo.person.address[1]=上海
demo.person.address[2]=广州
```
创建PersonController

```
import com.demo.config.PersonProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/person")
public class PersonController {

    @Autowired
    private PersonProperties personProperties;

    @GetMapping("/getInfo")
    public PersonProperties getPerson() {
        System.out.println(personProperties);
        return personProperties;
    }
}
```
启动类DemoApplication

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

浏览器访问 http://localhost:8290/person/getInfo 控制台输出  

```
PersonProperties{name='张三', age=18, address=[北京, 上海, 广州]}
```

#### 方式二：@ConfigurationProperties + @EnableConfigurationProperties
通过`@EnableConfigurationProperties(PersonProperties.class)`注解来指定需要用哪个实体Bean来装载配置信息，这时实体Bean就不用加`@Component`注解

Person类改为如下：

```
import org.springframework.boot.context.properties.ConfigurationProperties;

import java.util.List;

@ConfigurationProperties(prefix = "demo.person")
public class PersonProperties {
    ...
}
```
PersonController如下：

```
import com.demo.config.PersonProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/person")
@EnableConfigurationProperties(PersonProperties.class)
public class PersonController {

    @Autowired
    private PersonProperties personProperties;

    @GetMapping("/getInfo")
    public PersonProperties getPerson() {
        System.out.println(personProperties);
        return personProperties;
    }
}
```

浏览器访问 http://localhost:8290/person/getInfo 控制台输出  

```
PersonProperties{name='张三', age=18, address=[北京, 上海, 广州]}
```

#### 方式三：@Bean+@ConfigurationProperties
`@ConfigurationProperties`注解直接定义在`@Bean`的注解上，这时实体Bean就不用加`@Component`和`@ConfigurationProperties`这两个注解  

Person实体类改为如下：

```
import java.util.List;

public class PersonProperties {
  ...
}
```
PersonController改为如下：

```
import com.demo.config.PersonProperties;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/person")
public class PersonController {

    @Autowired
    private PersonProperties personProperties;

    @GetMapping("/getInfo")
    public PersonProperties getPerson() {
        System.out.println(personProperties);
        return personProperties;
    }
}
```
启动类改为如下：

```
import com.demo.config.PersonProperties;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class DemoApplication {

    @Bean
    @ConfigurationProperties(prefix = "demo.person")
    public PersonProperties mailProperties() {
        return new PersonProperties();
    }

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

浏览器访问 http://localhost:8290/person/getInfo 控制台输出  

```
PersonProperties{name='张三', age=18, address=[北京, 上海, 广州]}
```
> 参考 https://www.cnblogs.com/duanxz/p/4520571.html
