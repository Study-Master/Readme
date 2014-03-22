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

status:

1. unbooked - 距离考试时间还有超过3天＋未book

2. booked - 已经booked＋考试开始15分钟之前

3. closed － 未book＋距离考试时间不到3天／考试开始15分钟之后

4. finished － startExam之后记录

```JSON
{
	"event": "profile",
	"endpoint": "Server",
	"content": 
	{
		"account": "studymaster",
		"profile": 	
		{

			"courses" : 
			[
			{
				"code": "CZ2001",
				"name": "Java",
				"status": "unbooked",
				"start_time": "2014/04/01"
			},
			{
				"code": "CZ2002",
				"name": "Java2",
				"status": "booked",
				"start_time": "2014/03/03 00:00:00"
			},
		
			{
				"code": "CZ2006",
				"name": "Java6",
				"status": "closed",
				"start_time": "2014/03/03 00:00:00"
			},

			{
				"code": "CZ2003",
				"name": "Java3",
				"status": "finished",
				"start_time": "2014/03/03 00:00:00"
			}
			]
		}
	}
}
```


###Booking界面

Client请求：

```JSON
{
	"event": "booking",
	"endpoint": "Examinee",
	"content": {
		"code": "CZ2001",
		"account": "s"
	}

}
```

服务器返回：

```JSON
{
	"event": "booking",
	"endpoint": "Server",
	"content":  {
			"code": "CZ2001",
			"account": "s"
			"examTime": [{
					"start_time": "2014/03/03 00:00:00"
					}, 
					{...}, 
					{...}]//json array
	}
}
```
Client返回：
```JSON
{
	"event": "booked",
	"endpoint": "Examinee",
	"content": {
		"account": "s",
		"code": "CZ2006",
		"start_time": "2014/03/03 00:00:00"
	}
}
```
服务器返回：
```JSON
		{"event": "booked",
                 "endpoint": "Server",
                 "content": {
                     	"status": "success"
                 }
             }  
```
###考试界面
客户端请求：
```JSON
{
	"event": "exam_question",
	"endpoint": "Examinee",
	"content":
	{
		"code": "CZ0001",
		“account”: "s"
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
		"code": "CZ0001",
		"exam_pk": 3,
		"question_set": 
		[
		{
			"number": 1,
			"pk": 13,
			"type": "short",
			"question_content": 
			{
				"description": "What is the name of this course?",
				"answer": ""
			}
		},
		{
			"number": 2,
			"pk": 14,
			"type": "multi",
			"question_content": 
			{
				"description": "How much do you like this course?",
				"choices": 
				{
					"a": "very much",
					"b": "so so lor",
					"c": "not at all"
				},
				"answer": ""
			}
			//coz i put them in an arrayList, leave a "answer" there will be easier for us to send the answers
			//back to server
		}
			...
			..
			.
			//json string
		]
	}
}
```

考试结束后client side返回：
```JSON
{
	"event": "exam_question_answer",
	"endpoint": "Examinee",
	"content":
	{
		"code": "CZ0001",
		"exam_pk": 3,
		"question_set": 
		[
		{
			"number": 1,
			"pk": 13,
			"type": "short",
			"question_content": 
			{
				"description": "What is the name of this course?",
				"answer": "what the shit....can't finish lor..."
			}
		},
		{
			"number": 2,
			"pk": 14,
			"type": "multi",
			"question_content": 
			{
				"description": "How much do you like this course?",
				"choices": 
				{
					"a": "very much",
					"b": "so so lor",
					"c": "not at all"
				},
				"answer": "a"
			}
		}
			...
			..
			.
			//json string
		]
	}
}
```

server成功收到答案后的返回信息：
```JSON
{
	"event": "submission_message",
	"endpoint": "Server",
	"content":
	{
		"code": "CZ0001",
		"submission_status": "successful"
		//返回其他任何信息都视为 submission failed
	}
	
}
```

取消考试booking
```JSON
{
	"event": "cancel",
	"endpoint": "Examinee",
	"content":
	{
		"code": "CZ0001"
		"account": "s"
	}
	
}
```

系统回应取消booking
```JSON
{
	"event": "cancel",
	"endpoint": "Server",
	"content":
	{
		"code": "CZ0001",
		"account" "s",
		"status": "successful",
		"start_time": "2014/03/20 00:00:00"
	}
	
}
```

```JSON
{
	"event": "cancel",
	"endpoint": "Server",
	"content":
	{
		"code": "CZ0001",
		"account" "s",
		"status": "fail",
		"error": "XXXXX",
	}
	
}
```

###考试界面--考生提问，发送信息
客户端发送：
```JSON
{
	"event": "exam_chat",
	"endpoint": "Examinee",
	"content":
	{
		"acoount": "s",
		"exam_pk": 3,
		"system_time": "2014-03-20 14:33:07", 
				//SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		“msg”: "Teacher, I have a question."
	}
	
}
```
