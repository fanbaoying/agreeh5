#Agree HTML5 SDK

##支持的渠道
###移动页面支付
* 支付宝手机网页支付
* 银联网关支付
* 微信公众账号支付

---
##安装依赖
> npm run dev

---
##使用说明
###1.引入JS文件
```
<script src="/path/to/payWeb.js"></script>
```
###2.通过请求Agree网关获取支付凭据调用SDK
银联支付代码：
```
payWeb(data,'unionpay');
// 注意：
// 第一个参数是请求Agree网关成功后的数据
// 第二个参数第SDK识别银联支付渠道（这里必须是unionpay）
```

支付宝支付代码：
```
payWeb(data,'alipay');
// 注意：
// 第一个参数是请求Agree网关成功后的数据
// 第二个参数第SDK识别银联支付渠道（这里必须是alipay）
```

微信支付代码：
```
payWeb(data,'wechat');
// 注意：
// 第一个参数是请求Agree网关成功后的数据
// 第二个参数第SDK识别银联支付渠道（这里必须是wechat）
```

**注意：**
> 网关数据请求错误，SDK返回000001


###3.微信公众号接入注意事项
> 第一步：用户授权，获取code
window.location.href = 'https://open.weixin.qq.com/connect/oauth2/authorize?**appid**=APPID&**redirect_uri**=REDIRECT_URI&**response_type**=code&**scope**=snsapi_bas
e&**state**=STATE**#wechat_redirect** ';

参数说明：
| 参数              | 是否必须|  说明  |
| --------          | -----:  | :----: |
| appid             | 是      |   公众号的唯一标识   |
| redirect_uri      |   是    |   授权后重定向的回调链接地址，请使用urlEncode对链接进行处理   |
| response_type     |    是   |  返回类型，请填写code  |
| scope             |    是   |  应用授权作用域，请填写snsapi_base （不弹出授权页面，直接跳转，只能获取用户openid）|
| state             |    否   |  重定向后会带上state参数，开发者可以填写a-zA-Z0-9的参数值，最多128字节  |
| #wechat_redirect  |    是   |  无论直接打开还是做页面302重定向时候，必须带此参数  |


> 第二步：通过code换取网页授权access_token（以jquery为例）
**通过POST跨域请求获取openId**

```
$.ajax({
    type: 'POST',
    // 请求openId地址
    url: 'http://pay.soongrande.com/agree-pay-web-gateway/rewap/OPENID',
    data: {
        // 不同商户appid和secet的值是不同的
        appid: 'APPID',
        secret: 'SECRET',
        code: code
    },
    success: function (data) {
        var openId = data.result;
        // Agree网关数据请求
    }
```
