## 原作者项目地址：https://github.com/mixool/xrayku

### 以下内容根据原作者内容进行相应白话修改，并直接以自选IP的配置来表示，方便大家理解！

> 提醒： 滥用可能导致账户被BAN！！！   
  
* 使用[xray](https://github.com/XTLS/Xray-core)+caddy同时部署通过ws传输的vmess vless trojan shadowsocks socks等协议  
* 支持tor网络，且可通过自定义网络配置文件启动xray和caddy来按需配置各种功能  
* 支持存储自定义文件,目录及账号密码均为AUUID,客户端务必使用TLS连接  
  
[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/mixool/xrayku)  
  
# 服务端
点击上面紫色`Deploy to Heroku`，会跳转到heroku app创建页面，填上应用程序名、选择节点（美国或者欧洲）、自定义UUID码，点击下面deploy，几秒后搞定！    


# 客户端

## 1：xray

* 代理协议(CF Workers反代)：vless+ws+tls 或 vmess+ws+tls
* 地址：自选ip（如：icook.tw）
* 端口：443
* 默认UUID：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码)
* 加密：none
* 传输协议：ws
* 伪装类型：none
* 伪装host：****.workers.dev(CF Workers反代地址)
* path路径：/自定义UUID码-vless 或 /自定义UUID码-vmess    (注意：前有斜杠/)
* vmess额外id（alterid）：0
* 底层传输安全：tls
* 跳过证书验证：false

## 2：trojan-go+ws

* 地址：自选ip（如：icook.tw）
* 端口：443
* 密码：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码) 
* path路径：/自定义UUID码-trojan  (注意：前有斜杠/)
* SNI地址：****.workers.dev(CF Workers反代地址)
* 伪装host地址：****.workers.dev(CF Workers反代地址)

## 2：shadowsocks+ws+tls

* 服务器地址: 应用程序名.herokuapp.com
* 端口: 443
* 密码：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码) 
* 加密：chacha20-ietf-poly1305
* 插件程序：v2ray-plugin_windows_amd64.exe  //需将插件https://github.com/shadowsocks/v2ray-plugin/releases下载解压后放至shadowsocks同目录
* 插件选项: tls;host=应用程序名.herokuapp.com;path=/自定义UUID码-ss


## CloudFlare Workers反代代码

const SingleDay = '应用程序名.herokuapp.com'
const DoubleDay = '应用程序名.herokuapp.com'
addEventListener(
    "fetch",event => {
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)

