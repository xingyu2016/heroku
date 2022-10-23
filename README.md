
1、在浏览器复制链接   https://dashboard.heroku.com/new?template= 加上上传至Github的项目地址链接，回车进入Heroku参数设置界面



### CloudFlare Workers反代代码（可分别用两个账号的应用程序名（`path路径`、`协议`、`UUID`保持一致），单双号天分别执行，那一个月就有550+550小时（每个账号一个月免费使用550小时））
<details>
<summary>CloudFlare Workers单账户反代代码</summary>

```js
addEventListener(
    "fetch",event => {
        let url=new URL(event.request.url);
        url.hostname="appname.herokuapp.com";
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers单双日轮换反代代码</summary>

```js
const SingleDay = 'app0.herokuapp.com'
const DoubleDay = 'app1.herokuapp.com'
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
```
</details>

<details>
<summary>CloudFlare Workers每五天轮换一遍式反代代码</summary>

```js
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
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>CloudFlare Workers一周轮换反代代码</summary>

```js
const Day0 = 'app0.herokuapp.com'
const Day1 = 'app1.herokuapp.com'
const Day2 = 'app2.herokuapp.com'
const Day3 = 'app3.herokuapp.com'
const Day4 = 'app4.herokuapp.com'
const Day5 = 'app5.herokuapp.com'
const Day6 = 'app6.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        let day = nd.getDay();
        if (day === 0) {
            host = Day0
        } else if (day === 1) {
            host = Day1
        } else if (day === 2) {
            host = Day2
        } else if (day === 3){
            host = Day3
        } else if (day === 4) {
            host = Day4
        } else if (day === 5) {
            host = Day5
        } else if (day === 6) {
            host = Day6
        } else {
            host = Day1
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

三、客户端配置如下

IOS端小火箭就可以通吃，安卓端推荐V2rayNG或搭配Kitsunebi

协议：(vless/vmess/trojan)-ws

地址：app.heroku.com（自选IP/域名）

端口：443

用户ID/密码：自定义UUID

传输协议：ws

伪装host：app.heroku.com（workers或pages反代/自定义域）

路径path：/自定义UUID-协议开头两小写字母

传输安全：tls

SNI：app.heroku.com（workers或pages反代/自定义域）

其他设置保持默认即可！



shadowsocks-ws与socks5-ws推荐用Kitsunebi，配置简单，不需要plugin插件

协议：(shadowsocks/socks5)-ws

地址：app.heroku.com（自选IP/域名）

端口：443

shadowsocks密码：自定义UUID

shadowsocks加密方式：chacha20-ietf-poly1305(默认)

socks5用户名：空

socks5密码：空

传输协议：ws

伪装host：app.heroku.com（workers或pages反代/自定义域）

路径path：/自定义UUID-协议开头两小写字母

传输安全：tls

SNI(证书域名)：app.heroku.com（workers或pages反代/自定义域）

其他设置保持默认即可！

