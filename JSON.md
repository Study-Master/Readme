JSON简单说明
===

JSON：JavaScript 对象表示法（JavaScript Object Notation）。JSON 是存储和交换文本信息的语法。类似 XML。JSON 比 XML 更小、更快，更易解析。JSON使用类似Python中Dictionary的表示方法来表示数据，因此JSON具有人类可读性。

###一个简单的JSON例子

JSON使用花括号`{}`来作为开始和结束的标志，用双引号（或者单引号）引起一个Key和其对应的Value。如下便是一个简单的JSON例子：

```JSON
{
	"name": "Gong Yiwei",
	"age": 15,
	"university": "Nanyang Technological University"
}
```

在这个例子中，第一个引号表示key，第二个引号表示value（如果是整型，甚至可以不用引号）。两个值之间用冒号隔开。每一对值之间用逗号隔开。注意，最后一个值后面不跟随任何符号。

###JSON支持嵌套

JSON的值类型不一定是一个字符串，或是整数，甚至还可以是另一个JSON。即JSON的类型嵌套。比如下面就是一个JSON嵌套的例子：

```JSON
{
	"name": "Gong Yiwei",
	"age" : 15,
	"university": 	{
						"name": "Nanyang Technological University",
						"location": "Singapore"
					},
	"id": 10101
}
```

###JSON支持数组类型

JSON中还支持表示数组，数组使用`[]`引起，下面就是一个利用数组的JSON：

```JSON
{
	"name": "Gong Yiwei",
	"age" : 15,
	"university": 	{
						"name": "Nanyang Technological University",
						"location": "Singapore"
					},
	"id": 10101,
	"courses": 	[
					{
						"code": "CZ2001",
						"index": "10101"
					},
					{
						"name": "CZ2002",
						"index": "10102"
					}
				]
}
```

###JSON常用于数据交换

绝大多数语言都支持JSON的decode和encode，因此JSON常用于互联网的数据交换。使用JSON数据交换的时候，一般的步骤是创建JSON对象，将JSON扁平化（即将JSON对象转换为String，一般称这种类型的String为JSONString），向服务器发送扁平化后的JSONString，服务器解析，计算好结果后向客户端返回responce，当然一般是一个JSONString。

在C语言中，我们可以使用类似`printf`中的格式化String来创建一个JSONString，比如这就是个很好的例子，`"{\"name\": \"%s\"}"`。Java也可以使用这种C风格的格式化String来创建一个JSONString。需要注意，如果使用双引号，则要用反斜杠转义。由于JSON同时支持单引号，往往可以使用单引号代替双引号从而避免凡斜杠。

JSON官方提供了一个Java中处理JSON的类库，JSONObject类。利用JSONObject类可以很方便的创建一个JSON，并且将其扁平化。如下例子就创建了一个JSON，并扁平化成一个String：

```Java
protected String toJSON(String account, String passwd) {
		JSONObject session = new JSONObject();
		JSONObject user = new JSONObject();
		user.put("account", account);
		user.put("passwd", passwd);
		session.put("user", user);
		session.put("client", "client");
		return session.toString();
	}
```
在这个例子中如果`account = "studymaster"`，`passwd = "1234567890"`，这个函数会创建一个类似这样的JSONString，`"{'client':'client','user':{'passwd':'1234567890','account':'studymaster'}}"`

同样的JSONObject可以快速的解析一个JSONString。`JSONObject session = new JSONObject("{'client':'client','user':{'passwd':'1234567890','account':'studymaster'}}");`即可从JSONString中快速创建一个JSONObject。创建完毕后，我们便可以使用`String account = session.getString("account");`来获取JSONObject中的值。具体参考JSONObject的[官方文档](http://www.json.org/java/index.html)。关于其他语言的JSON操作也可以到JSON官网查看。