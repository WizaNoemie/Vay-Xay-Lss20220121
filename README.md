# 概述

本项目搭建好后没有第三方伪装网址

本项目用于在 Heroku 上部署 V2Ray/Xray WebSocket，在合理使用的程度下，本镜像不会因为大量占用资源而导致封号。

部署完成后，每次启动应用时，运行的 V2Ray/Xray 将始终为最新版本。

# 部署提示

|**注意事项**|
|**滥用可能导致账户被删除！！！**|
[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/WizaNoemie/Xay-Lss20220121)

# Xray VLESS
|**属性**|**值**|
|:------:|:----:|
|**地址**|优选IP<br>应用名.heroku.com|
|**端口**|443|
|**用户ID**|87b5c352-90d7-4ee1-9d1e-b03231c9fc53<br>**注意：务必使用自己创建的UUID。不要使用本项目中示范的UUID！**|
|**流控**|n/a|
|**加密**|none|
|**传输协议**|ws|
|**伪装类型**|none|
|**伪装域名**|xxxx.workers.dev(CF Workers反代地址)<br>应用名.heroku.com|
|**路径**|/ID-vless|
|**底层传输安全**|tls|
|**跳过证书验证**|false|
|**SNI**|xxxx.workers.dev(CF Workers反代地址)<br>应用名.heroku.com|

# Xray Trojan
|**属性**|**值**|
|:------:|:----:|
|**地址**|优选IP<br>应用名.heroku.com|
|**端口**|443|
|**用户ID**|87b5c352-90d7-4ee1-9d1e-b03231c9fc53<br>**注意：务必使用自己创建的UUID。不要使用本项目中示范的UUID！**|
|**流控**|n/a|
|**加密**|none|
|**传输协议**|ws|
|**伪装类型**|none|
|**伪装域名**|xxxx.workers.dev(CF Workers反代地址)<br>应用名.heroku.com|
|**路径**|/ID-trojan|
|**底层传输安全**|tls|
|**跳过证书验证**|false|
|**SNI**|xxxx.workers.dev(CF Workers反代地址)<br>应用名.heroku.com|

# Trojan Ws+Tls客户端支持状态
|**客户端**|**是否支持Trojan Ws+Tls？**|
|:--------:|:------------------------:|
|**2dust V2RayN<br>2dust V2RayNG**|是，需要电脑端4.27以上版本且有.net framework 6.0及更高版本<br>手机端敬请期待|
|**OpenWrt SSRPlus**|是|
|**OpenWrt SSRPlus**|是，需要最新版本passwall|
|~~**QV2Ray**~~|~~否~~|

# CloudFlare Workers反代代码（从以下3个示例中选择其中1个部署到CF Workers）

示例1：(适用单一应用的反代代码)
```
addEventListener(
  "fetch", event => {
    let url = new URL(event.request.url);
    url.host = "appname.herokuapp.com";
    let request = new Request(url, event.request);
    event.respondWith(
      fetch(request)
    )
  }
)
```

示例2：(适用单双日循环执行的反代代码)
```
const SingleDay = 'appname.herokuapp.com'
const DoubleDay = 'appname.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```

示例3：(适用多实例循环执行的反代代码)
```
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDate() % 5;
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4){
            host = Day4
        } else {
            host = Day0
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)

