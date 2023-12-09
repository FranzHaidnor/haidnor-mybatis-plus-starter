# haidnor-mybatis-plus-starter
1、集成 mybatis-plus，pagehelper，MySQL Connector, 多数据源
2、自动开始事务管理器注解 @EnableTransactionManagement

# 官方网址
[MyBatis-Plus (baomidou.com)](https://baomidou.com/)
[dynamic-datasource-spring-boot-starter: 基于 SpringBoot 多数据源 动态数据源 主从分离 快速启动器 支持分布式事务 (gitee.com)](https://gitee.com/baomidou/dynamic-datasource-spring-boot-starter)
[事务专栏 · dynamic-datasource · 看云 (kancloud.cn)](https://www.kancloud.cn/tracy5546/dynamic-datasource/2264598)

# Maven 依赖
包含多数据源配置
```xml
<dependency>
    <groupId>haidnor</groupId>
    <artifactId>haidnor-mybatis-plus</artifactId>
    <version>1.0</version>
</dependency>
```
不含多数据源
```xml
<dependency>
    <groupId>haidnor</groupId>
    <artifactId>haidnor-mybatis-plus</artifactId>
    <version>1.0</version>
    <exclusions>
        <exclusion>
            <groupId>com.baomidou</groupId>
            <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```


# YAML 配置
[MyBatis-Plus 配置文档](https://www.mybatis-plus.com/config/#基本配置)
单数据源配置

```yaml
spring:
  datasource:
    driverClassName: com.mysql.cj.jdbc.Driver
    name: dataSource
    url: jdbc:mysql://47.101.173.40:3306/wangxiang_test?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: root

mybatis:                                                      # Mybatis 配置类 org.mybatis.spring.boot.autoconfigure.MybatisProperties. 参考配置 https://mybatis.org/mybatis-3/zh/configuration.html#
  mapperLocations: classpath:/mapper/**/*.xml                 # 扫描 Mapper 接口对应的 XML 文件目录
  configuration:                                              # https://mybatis.org/mybatis-3/zh/configuration.html#settings
    mapUnderscoreToCamelCase: true                            # 是否开启驼峰命名自动映射,即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn, 默认 false

mybatis-plus:
    configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl # 打印SQL日志
  
pagehelper:                                                   # Mybatis 分页插件配置
  helper-dialect: mysql
  reasonable: false
  support-methods-arguments: true
  params: count:countSql
```

多数据源配置
[多数据源配置文档](https://gitee.com/baomidou/dynamic-datasource-spring-boot-starter)
```yaml
spring:
  datasource:
    dynamic:
      primary: vip   # 设置默认的数据源或者数据源组,默认值即为master
      strict: true   # 严格匹配数据源,默认false. true未匹配到指定数据源时抛异常,false使用默认数据源
      datasource:
        vip:
          url: jdbc:mysql://192.168.12.211:3306/xl_vip?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
          username: root
          password: root
        xuanle:
          url: jdbc:mysql://192.168.12.211:3306/hx_zxqy?useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
          username: root
          password: root

mybatis:                                                      # Mybatis 配置类 org.mybatis.spring.boot.autoconfigure.MybatisProperties. 参考配置 https://mybatis.org/mybatis-3/zh/configuration.html#
  mapperLocations: classpath:/mapper/**/*.xml                 # 扫描 Mapper 接口对应的 XML 文件目录
  configuration:                                              # https://mybatis.org/mybatis-3/zh/configuration.html#settings
    mapUnderscoreToCamelCase: true                            # 是否开启驼峰命名自动映射,即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn, 默认 false

mybatis-plus:
    configuration:
        log-impl: org.apache.ibatis.logging.stdout.StdOutImpl # 打印SQL日志
  
pagehelper:                                                   # Mybatis 分页插件配置
  helper-dialect: mysql
  reasonable: false
  support-methods-arguments: true
  params: count:countSql
```

# SpringBoot 启动类配置
在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹：
```java
@SpringBootApplication
@MapperScan("cn.haidnor.samples.mapper")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
