## 烧麦财播微服务初始化说明
#### 当前版本
> - SpringBoot版本  --> 1.5.8
> - SpringCloud版本 --> Dalston.SR4

### 创建项目规范

#### 类型划分
名称| 说明
---|---
base | 依赖模块
server | 基础设施
service | 业务模块
client | 接入层/入口

#### 推荐使用idea创建SpringBoot项目

```
具体步骤：File --> New --> Module --> Spring Initializr --> 填写项目信息--> 勾选需要组件 --> 选择目录，填写文件夹名 --> Finish
```

#### 命名方式：
> smcb-{type}-{name} 
###### 例如：

```
- smcb-base-model(model依赖)
- smcb-server-eureka(eureka服务中心)
- ...
```
#### 端口分配说明
> 可[参考此表](http://note.youdao.com/noteshare?id=752cd03ce703cef05b1661319b470272)，或@Forest

#### 包路径规范：

```
com.smcb.{type}.{name}  例如-->com.smcb.base.model
```
#### 配置说明:
> 推荐使用yml格式文件,以bootstrap.yml为初始配置,application.yml为常规配置文件，动态配置项请使用配置中心存放读取


#### 连接服务中心配置
```
eureka:
  client:
    service-url:
      defaultZone: ${SMCB_EUREKA:http://172.30.1.62:2100/eureka/}
  instance:
    prefer-ip-address: true
    instance-id: ${spring.cloud.client.ipAddress}:${server.port}
```
#### 日志引入
- 配置文件添加
```
logging:
  config: classpath:logback.xml
```
- 放置`logback.xml`
- 修改`logback.xml` 中的`APPID`和`LOG_PATH`对应模块名称和路径

#### API文档引入
> 这里推荐使用[apidocjs](http://apidocjs.com/)
- 依赖node.js环境
- 程序根目录输入命令`apidoc -i src` 就会在当前生成doc文档
- 具体文档参考官网
- 配置文件`apidoc.json`示例:
```
{
    "name": "运营活动接入层 API",
    "version": "1.0.0",
    "description": "烧麦财播 运营活动接入层",
    "title": "运营活动接入层 API",
    "url": "https://sitgw.shaomaicaibo.com/smcb-client-act",
    "sampleUrl": "https://sitgw.shaomaicaibo.com/smcb-client-act"
}

```

#### 其它支持
1. 支撑监控体系，请查看[监控体系接入](https://github.com/lmhaoye/smcbdoc/blob/master/%E7%9B%91%E6%8E%A7%E4%BD%93%E7%B3%BB%E6%8E%A5%E5%85%A5.md)
2. 需要配置中心化，请查看[配置中心使用说明](https://github.com/lmhaoye/smcbdoc/blob/master/%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)
3. 开发涉及服务之间互相调用，请参考[Feign使用说明](https://github.com/lmhaoye/smcbdoc/blob/master/Feign%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E.md)
4. 当前服务一览，[点击查看](https://github.com/lmhaoye/smcbdoc/blob/master/service.md)