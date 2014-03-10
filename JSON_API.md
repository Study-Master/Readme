JSON API
===


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
				}
}
```
如果登录失败，服务器返回
```JSON
{
	"event": "login",
	"endpoint": "Server",
	"content": 	{
					"status": "failed",
					"code": "reason code",
					"reason": "reason"
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
					"time": "system_time"
				}
}
```

服务器收到心跳包返回
```JSON
{
	"event": "pong",
	"endpoint": "Server",
	"content": 	{
					"status": "success",
					"time": "server_time"
				}
}
```

###Home界面
客户端请求：
```JSON
{
	"event": "profile",
	"endpoint": "Java Client",
	"content": 
	{
		"account": "studymaster"
	}
}
```

服务器返回：
```JSON
{
	"event": "profile",
	"endpoint": "Server",
	"content": 
	{
		"account": "studymaster",
		"profile": 	
		{
			"some_profile": "",
			"courses" : [
			{
				"code": "CZ2001",
				"name": "Java",
				"status": "unbooked",
				"start_time": NULL
			},
			{
				"code": "CZ2002",
				"name": "Java2",
				"status": "booked",
				"start_time": "2014/03/03 00:00:00"
			}
			]
		}
	}
}
```

###ExamStart Event
```JSON
{
	"event": "examstart",
	"endpoint": "Server",
	"content": 
	{
		"code": "CZ2002",
		"name": "Java2",
		"status": "booked",
		"start_time": "2014/03/03 00:00:00"
	}
}
```


###Booking界面

Client请求：

```JSON
{
	"event": "booking",
	"endpoint": "Java Client",
	"code": "CZ2001",
	"name": "Java",
	"studentID": "123456"
}
```

服务器返回：

```JSON
Example
{
	"event": "booking",
	"endpoint": "Server",
	"studentID": "123456"
	"content":  {
			"code": "CZ2001",
			"name": "Java",
			"examTime": [{
					"start_time": "2014/03/03 00:00:00"
					"end_time": "2014/03/03 00:01:00"
					}, 
					{...}, 
					{...}]//json array
	}
}
```

###考试界面
客户端请求：
```JSON
{
	"event": "exam_question",
	"endpoint": "Java Client",
	"content":
	{
		"code": "CZ0001"
	}
	
}
```

服务器返回：
```JSON
{
	"event": "exam_question",
	"endpoint": "Server",
	"content":
	{
		"course_code": "CZ0001",
		"exam_questions": 
		[
		{
			"question_number": "1",
			"question_type": "short_answer_question",
			"question": "What is the name of this course?"
		},
		{
			"question_number": "2",
			"question_type": "short_answer_question",
			"question": "What is the name of the lecturer?"
			...
			..
			.
			//json string
		}
		]
	}
}
```
