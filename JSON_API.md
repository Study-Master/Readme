JSON API
===
乱写的JSON交换API。。。。。不要管我。。。。

###连接

###登录
Client请求：
```JSON
{
	"event": "login",
	"endpoint": "Java Client",
	"content": 	{
					"account": "studymaster",
					"password": "password in md5Hex",
					"time": "system_time"
				}
}
```
如果登录成功，服务器返回
```JSON
{
	"event": "login",
	"endpoint": "Server",
	"content": 	{
					"status": "success",
					"token": "token"
				}
}
```
如果登录失败，服务器返回
```JSON
{
	"event": "login",
	"endpoint": "Server".
	"content": 	{
					"status": "faild",
					"code": "reason code",
					"reason": "reasom"
				}
}
```

###长连接和心跳包
每三十秒发送一次心跳包声明在线
```JSON
{
	"event": "ping",
	"endpoint": "Java Client",
	"content": 	{
					"token": "token",
					"time": "system_time"
				}
}

服务器收到心跳包返回
{
	"event": "pong",
	"endpoint": "Server",
	"content": 	{
					"status": "success",
					"time": "server_time"
				}
}

###Home界面
客户端请求：
```JSON
{
	"event": "profile",
	"endpoint": "Java Client",
	"content": 	{
					"account": "studymaster",
					"token": "token"
				}
}

服务器返回：
```JSON
{
	"event": "profile",
	"endpoint": "Server",
	"content": 	{
					"account": "studymaster",
					"profile": 	{
									"some_profile": "",
									"courses" : {
													"courseinfo", "info"
												}
								}
				}
}