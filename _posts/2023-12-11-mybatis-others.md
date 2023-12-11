---
title: mybatis 配置
date: 2023-12-11 14:38:35 +0800
categories: ['Tool', 'Mybatis']
tags: ['Mybatis相关']
---

目录结构如下:

```text
└─com
    ├─uniago
    │  └─demo
    │      ├─controller
    │      ├─entity
    │      ├─mapper
    │      └─service
    │          └─impl
    └─jan
        └─demo
            ├─aspect
            ├─config
            ├─constant
            ├─controller
            ├─dao
            ├─exception
            ├─mapper
            ├─model
            ├─service
            ├─utils
            ├─vo
            └─DemoApplication.java(SpringBoot主启动类)
```


若Java Bean不在Spring Boot主启动类所在包下,需要通过`@ComponentScan`注解指定`basePackages`去扫描所有的Bean

```java
@SpringBootApplication
@ComponentScan({"com.jan", "com.uniago"})
@MapperScan(basePackages = { "com.jan.demo.dao", "com.uniago.demo.mapper" })
public class DemoApplication{
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```


application.yaml配置

```yaml
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.log4j2.Log4j2Impl
    map-underscore-to-camel-case: true
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath:com/jan/demo/mapper/**/*.xml
  type-aliases-package: com.uniago.demo.entity,;com.jan.demo.model
```


Spring Boot集成Mybatis需要扫描Mybatis的Mapper接口和Mapper.xml文件

- `@MapperScan`注解用于扫描Mapper接口,类型是数组,可以指定多个
- 指定Mapper文件,可以通过yaml中的`mapper-locations`,类型是数组,可以指定多个

Mybatis支持`type-aliases-package`指定包别名,类型是String, 但也可以指定多个包,需要使用`,;`指定,并且会自动加上`/**/*.class`后缀

可参考`org.mybatis.spring.SqlSessionFactoryBean`源码

```java
  private Set<Class<?>> scanClasses(String packagePatterns, Class<?> assignableType) throws IOException {
    Set<Class<?>> classes = new HashSet<>();
    String[] packagePatternArray = tokenizeToStringArray(packagePatterns,
        ConfigurableApplicationContext.CONFIG_LOCATION_DELIMITERS);
    for (String packagePattern : packagePatternArray) {
      Resource[] resources = RESOURCE_PATTERN_RESOLVER.getResources(ResourcePatternResolver.CLASSPATH_ALL_URL_PREFIX
          + ClassUtils.convertClassNameToResourcePath(packagePattern) + "/**/*.class");
      for (Resource resource : resources) {
        try {
          ClassMetadata classMetadata = METADATA_READER_FACTORY.getMetadataReader(resource).getClassMetadata();
          Class<?> clazz = Resources.classForName(classMetadata.getClassName());
          if (assignableType == null || assignableType.isAssignableFrom(clazz)) {
            classes.add(clazz);
          }
        } catch (Throwable e) {
          LOGGER.warn(() -> "Cannot load the '" + resource + "'. Cause by " + e.toString());
        }
      }
    }
    return classes;
  }
```
