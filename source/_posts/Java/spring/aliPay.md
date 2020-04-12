---
title: 支付宝支付
tags:
- 支付
---

一个简单的支付demo

<!--more-->

# 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.imooc</groupId>
    <artifactId>pay</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>pay</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.2.6.RELEASE</spring-boot.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>cn.springboot</groupId>
            <artifactId>best-pay-sdk</artifactId>
            <version>1.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.alipay.sdk</groupId>
            <artifactId>alipay-sdk-java</artifactId>
            <version>3.3.49.ALL</version>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.2.6.RELEASE</version>
            </plugin>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <configuration>
                    <overwrite>true</overwrite>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>8.0.19</version>
                    </dependency>
                    <dependency>
                        <groupId>org.mybatis</groupId>
                        <artifactId>mybatis</artifactId>
                        <version>3.4.6</version>
                    </dependency>
                    <dependency>
                        <groupId>org.mybatis.generator</groupId>
                        <artifactId>mybatis-generator-core</artifactId>
                        <version>1.4.0</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

</project>
```

这里使用mybatis generator插件来生成java代码

支付功能通过best-pay-sdk来完成
配置连接: https://github.com/Pay-Group/best-pay-sdk/blob/develop/doc/use.md

页面使用freemarker

# 代码

application.yml
```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: root
    url: jdbc:mysql://localhost:3306/mall?serverTimezone=UTC&useUnicode=true&characterEncoding=utf8&useSSL=false
  freemarker:
    template-loader-path: classpath:/templates
    suffix: .ftl

alipay:
  appId: 123456
  privateKey: "应用私钥"
  publicKey: "支付宝公钥"
  notifyUrl: http://553jtx.natappfree.cc/pay/notify
  returnUrl: http://553jtx.natappfree.cc/pay/hello
  sandBox: true

server:
  port: 8080
```

alipay的相关参数配置连接: https://openhome.alipay.com/platform/appDaily.htm?tab=info

点击RSA2(SHA256)密钥的设置按钮
![](rsa.png)
在相关页面下载支付宝开放平台开发助手
通过该工具生成密钥

generatorConfig.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <context id="DB2Tables" targetRuntime="MyBatis3">

        <!--        不再追加xml内容-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin" />

        <!-- JavaBean 实现 序列化 接口 -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>

        <!-- genenat entity时,生成toString -->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin" />

        <!-- 去除注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mall?serverTimezone=UTC&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false"
                        userId="root"
                        password="root">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <javaModelGenerator targetPackage="com.imooc.pay.pojo" targetProject="src/main/java">
            <property name="enableSubPackages" value="true" />
            <!--            <property name="trimStrings" value="true" />-->
        </javaModelGenerator>

        <sqlMapGenerator targetPackage="mappers"  targetProject="src/main/resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <javaClientGenerator type="XMLMAPPER" targetPackage="com.imooc.pay.dao"  targetProject="src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <table tableName="mall_pay_info" domainObjectName="PayInfo" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
    </context>
</generatorConfiguration>
```

数据库

```sql
CREATE TABLE `mall_pay_info` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) DEFAULT NULL COMMENT '用户id',
  `order_no` bigint(20) NOT NULL COMMENT '订单号',
  `pay_platform` int(10) DEFAULT NULL COMMENT '支付平台:1-支付宝,2-微信',
  `platform_number` varchar(200) DEFAULT NULL COMMENT '支付流水号',
  `platform_status` varchar(20) DEFAULT NULL COMMENT '支付状态',
  `pay_amount` decimal(20,2) NOT NULL COMMENT '支付金额',
  `create_time` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uqe_order_no` (`order_no`),
  UNIQUE KEY `uqe_platform_number` (`platform_number`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

点击maven工具中的mybatis-generator:generator生成相关java代码

由于此处微信不方便验证，所以只有支付宝的相关代码

config包

AlipayAccountConfig.java
这里直接将application中的参数注入到相应的变量中
```java
@Component
@Data
@ConfigurationProperties(prefix = "alipay")
public class AlipayAccountConfig {

    private String appId;

    private String privateKey;

    private String publicKey;

    private String notifyUrl;

    private String returnUrl;

    private boolean sandBox;
}
```

BestPayConfig.java
```java
@Component
public class BestPayConfig {

    @Resource
    private AlipayAccountConfig alipayAccountConfig;

    @Bean
    public BestPayService bestPayService() {

        AliPayConfig aliPayConfig = new AliPayConfig();
        aliPayConfig.setAppId(alipayAccountConfig.getAppId());
        aliPayConfig.setPrivateKey(alipayAccountConfig.getPrivateKey());
        aliPayConfig.setAliPayPublicKey(alipayAccountConfig.getPublicKey());
        aliPayConfig.setNotifyUrl(alipayAccountConfig.getNotifyUrl());
        aliPayConfig.setReturnUrl(alipayAccountConfig.getReturnUrl());
        aliPayConfig.setSandbox(alipayAccountConfig.isSandBox());
        BestPayServiceImpl bestPayService = new BestPayServiceImpl();
        bestPayService.setAliPayConfig(aliPayConfig);
        return bestPayService;
    }
}
```

这里使用的支付宝的沙箱环境, 默认为false
```java
aliPayConfig.setSandbox(alipayAccountConfig.isSandBox());
```

service包

IPayService.java

```java
public interface IPayService {

    PayResponse create(String orderId, BigDecimal amount);

    String asyncNotify(String notifyData);

}
```

PayService.java
```java
@Slf4j
@Service
public class PayService implements IPayService {
    @Resource
    private BestPayService bestPayService;

    @Override
    public PayResponse create(String orderId, BigDecimal amount) {
        PayRequest request = new PayRequest();
        request.setOrderName("快付钱");
        request.setOrderId(orderId);
        request.setOrderAmount(amount.doubleValue());
        request.setPayTypeEnum(BestPayTypeEnum.ALIPAY_PC);

        PayResponse response = bestPayService.pay(request);
        log.info("response={}", response);

        return response;
    }

    @Override
    public String asyncNotify(String notifyData) {
        //1. 签名检验
        PayResponse payResponse = bestPayService.asyncNotify(notifyData);
        log.info("payResponse={}", payResponse);

        //2. 金额校验（从数据库查订单）

        //3. 修改订单支付状态

        return "success";
    }
}
```

controller
```java
@Controller
@RequestMapping("/pay")
public class PayController {

    @Resource
    private PayService payService;

    @GetMapping("/create")
    public ModelAndView create(@RequestParam("orderId") String orderId,
                               @RequestParam("amount") BigDecimal amount) {
        Map<String, String> map = new HashMap<>();
        PayResponse response = payService.create(orderId, amount);
        map.put("body", response.getBody());
        return new ModelAndView("createForAlipayPc", map);
    }

// 异步通知
    @PostMapping("/notify")
    @ResponseBody
    public String asyncNotify(@RequestBody String notifyData) {
        return payService.asyncNotify(notifyData);
    }

    @GetMapping("/hello")
    @ResponseBody
    public String hello(){
        return "hello";
    }
}
```

templates包

createForAlipayPc.ftl
```html
<html>
<head>
    <meta charset="utf-8">
    <title>支付</title>
</head>
<body>
${body}

</body>
</html>
```

# 测试

链接: http://localhost:8080/pay/create?orderId=2233&amount=123&payType=ALIPAY_PC

![](pay.png)

扫码支付需要使用沙箱app
![](app.png)

支付成功后跳转到hello页面

# 说明

这里使用的是沙箱环境, 正式版需要创建相应的应用, 修改相应的参数即可
