### Feign使用说明
> 为规范微服务之间互相调用，推荐使用Feign方式
---
> svn目录 trunk\base\smcb-base-feign 增加对应接口目录即可
- 该项目为多模块项目，请注意相关依赖
- 命名使用


1. 项目增加依赖
```
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-feign</artifactId>
        </dependency>
```
2. 增加对应模块feign，例如：

```
        <dependency>
            <groupId>com.smcb</groupId>
            <artifactId>smcb-feign-third</artifactId>
            <version>1.2</version>
        </dependency>
```
3. 程序主类增加注解：`@EnableFeignClients`
4. 主类所在包必须为`com.smcb` 
5. 配置推荐增加：
```
        ribbon:
          ConnectTimeout: 3000
          ReadTimeout: 60000
          httpclient:
            enabled: false
          okhttp:
            enabled: true
```
启用`okhttp` 该库存在与 `smcb-base-util` 中

6. 例子可见`smcb-client-act`项目