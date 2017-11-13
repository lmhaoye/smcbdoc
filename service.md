
> 服务总表

序号| 服务|别名 | 端口|所在服务器 |路径(/data/web/)
---|---|--|---|---|---|
1|smcb-server-eureka|注册中心|12100|core-1/core-2|server/smcb-server-eureka.jar
2|smcb-server-config|配置中心|12110|core-1/core-2|server/smcb-server-config.jar
3|smcb-server-res|资源服务|12111|core-1|server/smcb-server-res.jar
4|smcb-server-sms|短信服务|12112|core-1|server/smcb-server-sms.jar
5|smcb-server-zuul|网关服务|12200|app-1/app-2|server/smcb-server-zuul.jar
6|smcb-client-app-vip|vip-APP接入层|12201|app-1/app-2|client/smcb-client-app-vip.jar
7|smcb-client-app-index|指标APP接入层|12202|app-1/app-2|client/smcb-client-app-index.jar
8|smcb-service-news|资讯服务|12204|app-1/app-2|service/smcb-service-news.jar
9|smcb-wx|微信程序|12206|app-1/app-2|smcb-wx/smcb-wx.jar
10|smcb-service-ak|练K服务|12300|app-1/app-2|service/smcb-service-ak.jar
11|smcb-service-pay|支付服务|12301|core-2|service/smcb-service-pay.jar
12|smcb-service-audio|音频服务|12302|core-2|service/smcb-service-audio.jar
13|smcb-service-message|消息服务|12303|core-2|service/smcb-service-message.jar
14|smcb-server-admin|服务管理|12101|core-2|server/smcb-server-admin.jar
15|smcb-service-ale|服务|12305|core-2|service/smcb-service-ale.jar
16|smcb-service-third|服务|12306|core-2|service/smcb-service-ale.jar

---
> 域名与服务对应

- wx.shaomaicaibo.com --> smcb-wx:12206
- gw.shaomaicaibo.com --> smcb-server-zuul:12200
