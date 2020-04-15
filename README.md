# weixin-xposed-server
微信http服务器形式的xposed模块，提供http api，让你可以控制微信主动发送消息

为了防止被人恶意调用接口。我增加了Google二次认证形式的安全校验，调用接口时，必须附上动态码，手机上的服务器校验通过后才会发送微信消息。因为是动态码，有时效性，就算被人拦截也不会有太大的影响。比直接传递密码安全多了。需要注意，因为是基于时间的动态码，调用方和服务方必须时间同步才行，至少不能相差太多。

使用方式就是配置Constants中的seed，调用方使用相同seed生成动态码，调用是附上（token字段），即可。

例如手机ip是192.168.0.3

POST方式访问```http://192.168.0.3:9000```

报文为：

```json
{
    "token": "504655",//换成自己的动态码
    "message": {
        "userId": "wxid_****", //接受方微信id
        "content": "Hello World" //消息内容，暂时只支持文本消息，注意微信对文本消息有长度限制，超过限制会发送失败
    }
}
```

另外，因为微信会调整不同版本手机app的api，这个xposed模块我是在微信[7.0.3](https://www.wandoujia.com/apps/596157/history_v1400)版本下使用，其他版本不知道是否适用。

## TODO

1. 增加微信新消息转发（来新消息通过http请求转发到自己系统中）

2. 增加微信联系人获取接口（微信收到信息时，chatroom是对方的微信id，需要通过通讯录才能转换为看得懂得样子） 

3. 相关参数可配置（目前seed和端口号9000都是写在代码里面，不能修改）


