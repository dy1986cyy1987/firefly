## ***firefly API开发计划【Beta版本】***

```
一期开发【赛事、约战】API
```

### G1 私钥全局约定

``` 
私钥：Mpks1$01098#AlsNj 

以下用 `$private_key` 代替这个私钥值
```	
	
### G2 Api或H5页面URL全局约定
	
	```
	列表页URL：URI中包含lists
	
	详情页URL：URI中包含details
	
	动作URL：URI中包含activities
	
	分享URL：URI中包含share
	
	收藏URL：URI中包含collect
	
	用户信息APi: URI中包含user
	```

### G3 TOKEN生成规则全局约定
	
```
	`对所有的参数名及参数值进行校验`
	
	`TOKEN生成与请求参数的顺序有关，及时统一的参数name和参数值，但是顺序不一致，
	 那么生成的token也不一致。例如请求参数m=act&param=123和param=123&m=act这两
	 种请求的TOKEN是不一样的`
	
	`以m=act&param=123为例 
	 TOKEN = md5(Mpks1$01098#AlsNj + m + act + param + 123) 
	       = md5(Mpks1$01098#AlsNjmactparam123)`

	`以param=123&m=act为例 
	 TOKEN = md5(Mpks1$01098#AlsNj + param + 123 + m + act) 
	       = md5(Mpks1$01098#AlsNjparam123mact)`
```

### G4 所有接口返回格式全局约定:

```
成功时返回数据格式(两种)：
{
	"status": 0,
	"data" : {}
}

或者

{
	"status": 0,
	"data" : []
}

或者失败返回：
{
	"status" : $error_code, //非0
	"error_msg" : {
		// todo 错误信息json
	},
	"data" : {}
}

请求成功时status是0，data为成功后数据；其他不是0的均请求均为错误码，对应得data为错误信息
```

### ***登陆API***

```
登陆接口使用食人、tt登陆那一套接口。登陆Api返回的数据格式不遵循上面的全局约定，因为它是老接口。

在原有的tt登陆Api基础上，只需要新增参数参数from_type=netbar即可，出参结果与原接口完全一致。老的登陆接口参数在此不作描述
```

### ***注册API***

```
注册接口也使用食人、tt登陆那一套接口。注册Api返回的数据格式不遵循上面的全局约定，因为它也是老接口。

在原有的tt注册Api基础上，只需要新增参数参数from_type=netbar即可，出参结果与原接口完全一致。老的注册接口参数在此不作描述
```

### ***<赛事、约战API>***

```
A 与`赛事、约战`直接相关API:
	1. 赛事信息列表页API
	2. 赛事报道列表页API
	3. 赛事详情API
	4. 比赛详情Page(H5)
	5. 赛事分享Page(H5)
	6. 赛事收藏API
	7. 取消赛事收藏API
	8. 赛事报道详情Page(H5)
	9. 赛事报道收藏API
	10. 取消赛报道事收藏API
	11. 获取不服来战列表Api
	12. 发起约战Api
	13. 获取约战详情Api(根据约战id)
	14. 加入约战Api
	15. 收藏约战Api
	16. 取消收藏约战Api
	17. 分享约战H5页面

B 与`赛事、约战`间接相关API: 
	1. 赛事列表页筛选项API(客户端在使用A1 Api之前需要先拉取筛选条件)
	2. 已报战队列表页筛选项API(B3中需要先行调用该接口值)
	3. 查看已报战队列表API
	4. 查看战队详情API
	5. 加入战队API(入口战队详情页)
	6. 查看战队申请列表API(入口在战队详情页)
	7. 解散战队Api(只有`队长`才有权限解散)
	8. 分享战队Page(H5页面，入口战队详情页)
	9. 获取某一赛事的特定网吧列表Api(依赖赛事match_id)
	10. 获取某一赛事的比赛场次列表Api(依赖赛事match_id)
	11. 获取某一赛事的区服列表Api(依赖赛事match_id)
	12. 创建战队Api

C `报名Api`
	1. 个人报名参加赛事Api
	2. 团队报名参加赛事Api
```

### ***<公共Api>***

```
P1 根据user_id获取网吧用户信息Api
P2 获取身份认证类型Api
P3 获取筛选项中所有游戏类型Api
P4 获取筛选项中所有游戏类型Api
```


- ***A1 赛事信息列表页API***

```
请求URL: http://221.228.215.97:9020/lists/getMatchInfoLists/

请求方式：POST
```  
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|page|分页第几页|integer|否|默认1|
|rows|每页取多少条数据|integer|否|默认20|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|
|game_id|游戏类型id|integer|否|检索条件中的游戏类型id|
|status|赛事状态|interger|否|1：报名中 2：报名已截止 3：赛事已结束 默认1|
|type|线上还是线下|integer|否|1：线上 2：线下|
|lat|经度|float|否|地理位置经度|
|lng|经度|float|否|地理位置纬度|
|city_id|区|integer|否|按城市检索|

```
token生成规则：

token = md5(私钥 + 所有的参数$key + $value相加)
如请求的uri为 rows=11&status=1&type=1，那么
token = md5(Mpks1$01098#AlsNjrows11status1type1)
```

```
接口成功时返回数据类型：
{
	"status":0,
	"data":
	{
		"page_info":
		{
			"page":1,
			"total":1001,
			"rows":20,
			"find":20
		}, // 分页信息
		
		"docs": // 请求成功的数据
		[
			{
				"match_id":1111, // 赛事id
				"match_name":"Dota2世界冠军争夺赛第二场", // 赛事名称
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事图
				"start_time":"2015-12-11", // 开始时间
				"end_time":"2015-12-20",   // 结束时间
				"status":"赛事已结束",       // 赛事状态
				"status_id":3, // 赛事状态id,与status对应
			}, 
			
			{
				"match_id":4444,
				"match_name":"LOL世界冠军争夺赛第二场",
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事图片
				"start_time":"2015-12-11", // 开始时间
				"end_time":"2015-12-20",   // 结束时间
				"status":"赛事已结束",       // 赛事状态
				"status_id":3, // 赛事状态id,与status对应
				"publish_time":"2015-12-10", // 赛事发布时间
			},
			
			...
			
			{
				"match_id":2222,
				"match_name":"DOTA世界冠军争夺赛第二场",
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事图片
				"start_time":"2015-12-11", // 开始时间
				"end_time":"2015-12-20",   // 结束时间
				"status":"赛事已结束",       // 赛事状态
				"status_id":3, // 赛事状态id,与status对应
				"publish_time":"2015-12-10", // 赛事发布时间
			},
		]
	}
}

接口失败时返回数据格式：
{
	"status" : $error_code,
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data" : {}
}
```

----

- ***A2 赛事报道列表页API***

```
私钥：Njs

请求URL：http://221.228.215.97:9020/lists/getMatchReportLists/

请求方式：POST
```  
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|page|分页第几页|integer|否|默认1|
|rows|每页取多少条数据|integer|否|默认20|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|
|game_id|游戏类型id|integer|否|检索条件中的游戏类型id| 

```
token生成规则：
token = md5(私钥 + 所有的参数$key + $value相加)
如请求的uri为 key1=111&msg=typeof&type=101，那么token = md5(Mpks1$01098#AlsNjkey1111msgtypeoftype101)
```

```
接口成功时返回数据类型：
{
	"status":0,
	"data":
	{
		"page_info":
		{
			"page":1,
			"total":1001,
			"rows":20,
			"find":20
		}, // 分页信息
		
		"docs":
		[
			{
				"match_report_id":1111, // 赛事报道id
				"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事报道图片,可能没有，也可能一张，也可能多张
				"time":"2015-12-11 14:22", // 开始时间
				"view_count":1001, // 多少人看过
				"recomment_count":54, // 评论数
			}, 
			
			{
				"match_report_id":2222, // 赛事报道id
				"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事报道图片,可能没有，也可能一张，也可能多张
				"time":"2015-12-11 14:22", // 开始时间
				"view_count":1001, // 多少人看过
				"recomment_count":54, // 评论数
			}, 	
					
			...
			
			{
				"match_report_id":3333, // 赛事报道id
				"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
				"img_urls":[
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
					http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				], // 赛事报道图片,可能没有，也可能一张，也可能多张
				"time":"2015-12-11 14:22", // 开始时间
				"view_count":1001, // 多少人看过
				"recomment_count":54, // 评论数
			}, 
		]
	}
}

接口失败时返回数据格式：
{
	"status" : $error_code,
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data" : {}
}
```

-----

- ***A3 赛事详情API***

```
请求URL： http://221.228.215.97:9020/details/getMatchDetail/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|
|match_id|赛事id|integer|是|必须传递赛事id，服务方会根据match_id查找详情|

```
token生成规则：
token = md5(私钥 + token以外的所有参数$key + $value相加)
如请求的uri为 match_id=222，那么token = md5(Mpks1$01098#AlsNjmatch_id222)
```

```
成功时返回接口数据格式：
{
	"status":0, //只有0时才能请求到正确的数据
	"data":{
		"city_name":"上海", // 赛事举办地点，这里有多场且不在同一个地方该怎么定义？？？
		"match_name":"赛事名称",
		"match_status":"赛事状态【报名中】",
		"img_urls":[
			http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
		],
		"start_time":"2015-12-11",
		"end_time":"2015-12-20",
		"places":[
			{
				"match_order":1, // 场次
				"place_order_desc":"第一场", // 场次描述
				"netbar_name":"黑超网吧", // 网吧名字
				"start_time":"2015-12-11 09:00",
				"end_time":"2015-12-11 16:00",
			}, // 比赛地点1
			{
				"match_order":2, // 场次
				"place_order_desc":"第二场", // 场次描述
				"netbar_name":"黑超网吧", // 网吧名字
				"start_time":"2015-12-11 09:00",
				"end_time":"2015-12-11 16:00",
			}, // 比赛地点2
			
			...
			
			{
				"match_order":100,
				"place_order_desc":"第N场",
				"address":"云海之家网吧",
				"start_time":"2015-12-12 08:00",
				"end_time":"2015-12-12 15:00",
			}, // 比赛地点N
		], // 比赛地点，数组格式，至少会有一个元素
		
		"members":[
			"total_num":123, //该赛事已报名人数
			"info":[
				{
					"member_icon":"$img_url", // 字符串 用户icon url
					"user_id":"$user_id" // 用户id
				},
				
				... 
				
				{
					"member_icon":"$img_url", // 字符串 用户icon url
					"user_id":"$user_id" // 用户id
				},
			]
		], // 已报名人员列表(包含团队赛和个人赛报名所有人员)
	}
}

接口失败时返回数据格式：
{
	"status" : $error_code,
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data" : {}
}
```

-----

- ***A4 比赛详情Page(完全由运营人员输入)***

```
H5 page url： http://221.228.215.97:9020/details/getMatchActivityDetail/{$match_id}

$match_id为赛事id

PS：后台会为运营人员提供一个编辑工具箱，对于每一场比赛，运营必须为它手工填写比赛详情。
```

----

- ***A5 赛事分享Page***

```
赛事分享不需要验证，客户端分享赛事Url:
	
http://221.228.215.97:9020/share/match/{$match_id}

其中$match_id为要分享的赛事的id
```

----

- ***A6 赛事收藏API***

```
赛事收藏URL：http://221.228.215.97:9020/collect/match

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|待收藏的赛事id|
|user_id|用户id|ineger|是|用户id，即谁要收藏|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
token生成规则：
token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为match_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + match_id11 + user_id22)
```

```
接口请求成功时返回参数
{
	"status":0, 
	"data":"$success_msg"
}

接口请求失败时返回参数
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A7 取消赛事收藏API***

```
赛事收藏URL：http://221.228.215.97:9020/collect/cancel

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|待取消收藏的赛事id|
|user_id|用户id|ineger|是|用户id，即谁要收藏|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

- token生成规则

```
token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为match_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + match_id11 + user_id22)
```

```
接口请求成功时返回参数
{
	"status":0, 
	"data":"$success_msg"
}

接口请求失败时返回参数
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A8 赛事报道详情H5 Page***

```
赛事报道详情页不需要身份验证与参数校验

H5页面URL: http://221.228.215.97:9020/details/matchReport/{$report_id}

$report_id为赛事报道的报道id
```

---

- ***A9 赛事报道收藏API***

```
赛事收藏URL：http://221.228.215.97:9020/collect/matchReport

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|report_id|赛事id|integer|是|待收藏的赛事id|
|user_id|用户id|ineger|是|用户id，即谁要收藏|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
token生成规则：
token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为report_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + report_id11 + user_id22)
```

```
接口请求成功时返回参数
{
	"status":0, 
	"data":"$success_msg"
}

接口请求失败时返回参数
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A10 取消赛事报道收藏API***

```
赛事收藏URL：http://221.228.215.97:9020/collect/cancelReport

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|report_id|赛事报道id|integer|是|待取消的赛事报道id|
|user_id|用户id|ineger|是|用户id，即谁要取消收藏赛事报道|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

- token生成规则

```
token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为report_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + report_id11 + user_id22)
```

```
接口请求成功时返回参数
{
	"status":0, 
	"data":"$success_msg"
}

接口请求失败时返回参数
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A11 获取不服来战列表Api***

```
Api作用：拉取约战列表页数据

Api地址：http://221.228.215.97:9020/lists/getDateFightLists/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|page|分页第几页|integer|否|默认1|
|rows|每页取多少条数据|integer|否|默认20|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|
|game_id|游戏类型id|integer|否|检索条件中的游戏类型id|
|report_type|发起者类型|integer|否|1：个人发起 2：网吧发起：3：我发起的 4：quan'bu|
|status|赛事状态|interger|否|1：报名中 2：已开始 3：全部 默认3|
|type|线上还是线下|integer|否|1：线上 2：线下 3：不区分 目前这个字段没有加入筛选项中|

```
`TOKEN生成`遵循全局约定

例如某次请求参数为：rows=20&page=2&report_type=1，则

token = md5(Mpks1$01098#AlsNjrows20page2report_type1)
```

```
Api请求成功返回数据：
{
	"status" : 0,
	"data" : [
		{
			"base_info" : {
				"id" : 1, //约战id
				"logo_imgs" : [
					"http://www.pic.com/p/ms/1.jpg",
					...
					"http://www.pic.com/p/ms/3.jpg",
				], //约战logo，数组，至少有一个图片地址
				"title" : "xxxx的约战title", //约战title
				"netbar_id" : 1, //网吧id
				"netbar_name" : "网吧名字",
				"game_name" : "游戏名字",
				"area_name" : "区名",
				"server_name":"服名",
				"time" : "12月16日 11：30", //约战时间
				"type" : "线上", //线下线上id
				"type_id" : 1, //线下线上id
				"status" : "报名中", //报名状态
				"status_id" : 1, //报名状态id
			},
			
			"join_users" : {
				"joined_num" : 3, //已加入约战人数
				"limit" : 5, //最多5人参加约战
				"users" : [
					{
						"user_id" : 1, //参加约战者id
						"user_name" : "参加约战者姓名",
						"user_icon" : "参加约战者的icon"
					},
					
					...
					
					{
						"user_id" : 1, //参加约战者id
						"user_name" : "参加约战者姓名",
						"user_icon" : "参加约战者的icon"
					},	
				]
			}
		},
		
		... 
		
		{
			"base_info" : {
				"id" : 1, //约战id
				"logo_imgs" : [
					"http://www.pic.com/p/ms/1.jpg",
					...
					"http://www.pic.com/p/ms/3.jpg",
				], //约战logo，数组，至少有一个图片地址
				"title" : "xxxx的约战title", //约战title
				"netbar_id" : 1, //网吧id
				"netbar_name" : "网吧名字",
				"game_name" : "游戏名字",
				"area_name" : "区名",
				"server_name":"服名",
				"time" : "12月16日 11：30", //约战时间
				"type" : "线上", //线下线上id
				"type_id" : 1, //线下线上id
				"status" : "报名中", //报名状态
				"status_id" : 1, //报名状态id
			},
			
			"join_users" : {
				"joined_num" : 3, //已加入约战人数
				"limit" : 5, //最多5人参加约战
				"users" : [
					{
						"user_id" : 1, //参加约战者id
						"user_name" : "参加约战者姓名",
						"user_icon" : "参加约战者的icon"
					},
					
					...
					
					{
						"user_id" : 1, //参加约战者id
						"user_name" : "参加约战者姓名",
						"user_icon" : "参加约战者的icon"
					},	
				]
			}
		}
	]
}

接口请求失败返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A12 发起约战Api***

```
请求地址：http://221.228.215.97:9020/activities/createDateFight/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|约战发起者id|integer|是|约战发起者的用户id,即登陆id|
|title|约战名字|string|是|约战名字，不能为空|
|game_id|游戏id|integer|是|约战游戏id|
|time|约战时间|string|是|约战时间，不能比当前时间小|
|type|线上、线下|integer|是|约战方式，1：线上 2：线下|
|netbar_id|网吧id|integer|否|线下约战时必需选择一个网吧|
|limit|约战人数限制|integer|是|人数限制|
|intro|约战介绍|string|否|约战介绍，包含规则、奖品等|
|contact|联系方式|string|是|联系方式，客户端必须传递|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为report_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + report_id11 + user_id22)
```

```
约战Api请求成功数据格式：
{
	"status" : 0,
	"data" : {
		"datefight_id" : 1, //成功约战id,
		"group_id" : 2, //约战群id
		"msg" : "success", //成功提示信息
	}
}

接口请求失败返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A13 获取约战详情Api(根据约战id)***

```
Api功能：根据约战id获取约战详情

Api地址：http://221.228.215.97:9020/details/getDateFightInfoById/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|id|约战id|integer|是|约战id，客户端传递，列表页返回的有这个id|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
`TOKEN`遵循全局约定

token = md5(私钥 + token以外的所有参数$key + $value相加)
如果请求的URI为report_id=11&user_id=22，那么
token = md5(Mpks1$01098#AlsNj + report_id11 + user_id22)
```

```
Api请求成功时返回数据：
{
	"status" : 0,
	"data" : {
		"small_logo" : "",
		"big_log" : "",
		"status" : "报名中", //报名状态
		"title" : "xxxx的约战", //约战title
		"server" : "全服", //服务器
		"game" : "英雄联盟", //游戏项目
		"time" : "12月16日 11：30", //约战时间
		"address" : "地点", //约战地址，即网吧地址
		"intro" : "约战介绍",
		"limit" : 50, //约战人数限制
		"joined_num" : 15, //已加入约战人数
		"members" : [
			{
				"user_name" : "张三",
				"icon" : "http://www.xzj.com/pic/mgs/1.jpg"
			},
			
			...
			
			{
				"user_name" : "张三",
				"icon" : "http://www.xzj.com/pic/mgs/1.jpg"
			}
		]
	}
}

请求失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A14 加入约战Api***

```
Api功能：根据约战id，用户id加入到约战

Api地址：http://221.228.215.97:9020/activities/joinDateFight/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|用户id|integer|是|登陆用户的用户id|
|id|约战id|integer|是|约战id|
|token|认证token|string|是|token按照参数顺序md5生成|

```
`TOKEN生成`：

例如一次请求URI参数为user_id=1&id=2，那么

token = md5(Mpks1$01098#AlsNjuser_id1id2)
```

```
Api请求成功时返回数据：
{
	"status" : 0,
	"data" : {
		"msg" : "success"
	}
}

请求失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A15 收藏约战Api***

```
Api请求地址：http://221.228.215.97:9020/collect/dateFight/

Api请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|用户id|integer|是|待收藏的用户id|
|id|约战id|integer|是|带收藏的约战id|
|token|认证token|string|是|token按照参数顺序md5生成|

```
`TOKEN生成`：

例如一次请求URI参数为user_id=1&id=2，那么

token = md5(Mpks1$01098#AlsNjuser_id1id2)
```

```
Api请求成功时返回数据：
{
	"status" : 0,
	"data" : {
		"msg" : "success"
	}
}

请求失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A15 取消收藏约战Api***

```
Api请求地址：http://221.228.215.97:9020/collect/disDateFight/

Api请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|用户id|integer|是|待取消收藏的用户id|
|id|约战id|integer|是|带取消收藏的约战id|
|token|认证token|string|是|token按照参数顺序md5生成|

```
`TOKEN生成`：

例如一次请求URI参数为user_id=1&id=2，那么

token = md5(Mpks1$01098#AlsNjuser_id1id2)
```

```
Api请求成功时返回数据：
{
	"status" : 0,
	"data" : {
		"msg" : "success"
	}
}

请求失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***A17 分享约战H5页面***

```
PS: 分享约战页面是H5统一制作，页面内容

页面地址：http://221.228.215.97:9020/share/dateFight/{$id}
```

---

- ***B1 赛事列表页筛选项API***

```
赛事列表页筛选项Api URL： http://221.228.215.97:9020/filter/matchInfoList/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
`TOKEN生成：`

token = md5($private_key + YYYY-mm-dd)
如请求该接口时当天是2016-11-23，那么
token = md5(Mpks1$01098#AlsNj + 2016-11-23)
      = md5(Mpks1$01098#AlsNj2016-11-23),
这里主要是防止第三方来盗取我们的数据，假如我们的数据有更新    
```

```
接口请求成功时返回数据格式：
{
	"status" : 0,
	"data"   : 
	{
		"games" : [
			{
				"game_name" : "dota2",
				"game_id"   : 100 ,
			},
			
			...
			
			{
				"game_name" : 英雄联盟",
				"game_id"   : 102 ,
			},
		], // 游戏
		
		"status" : [
			{
				"code" : 1,
				"desc" : "报名中"
			},
			{
				"code" : 2,
				"desc" : "报名截止",
			},
			{
				"code" : 3,
				"desc" : "赛事结束",
			},
		], // 比赛状态
		
	   "address" : [
	   	{
	   		"city_id" : 11, 
	   		"city_name" : "上海",
	   	},
	   	{
	   		"city_id" : 12, 
	   		"city_name" : "杭州",
	   	},
	   	
	   	...
	   	
	   	{
	   		"city_id" : 100, 
	   		"city_name" : "北京",
	   	}
	   ] // 地区
	}
}

失败时返回数据：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B2 已报战队列表页筛选项Api***

```
Api请求URL：http://221.228.215.97:9020/filter/getTeams/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id|
|kw|搜索内容|string|否|可搜索网吧名字、战队名字等，一期不做，此处预留|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
`token生成规则：`

token = md5(私钥 + token以外的所有参数$key + $value相加)

如果请求的URI为match_id=11，那么token = md5(Mpks1$01098#AlsNj + match_id11)；

如果请求的URI为match_id=11&kw=黑超网吧，那么token = md5(Mpks1$01098#AlsNj + match_id11 + 黑超网吧) = md5(Mpks1$01098#AlsNjmatch_id11黑超网吧)；
```

```
B2接口成功时返回数据：
{
	"status" : 0,
	"data" : [
		{
			"netbar_id" : 11, //网吧id
			"netbar_name" : "网吧名字",
		},
		
		...
		
		{
			"netbar_id" : 2222, //网吧id
			"netbar_name" : "网吧名字",
		},
	]
}

失败时返回数据：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B3 已报战队列表页Api***

```
请求Api URL：http://221.228.215.97:9020/lists/getFightTeams/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id|
|kw|搜索内容|string|否|可搜索网吧名字、战队名字等，一期不做，此处预留|
|netbar_id|网吧id|integer|否|筛选指定网吧的战队|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
`token生成规则：`

token = md5(私钥 + token以外的所有参数$key + $value相加)

如果请求的URI为match_id=11，那么token = md5(Mpks1$01098#AlsNj + match_id11)；

如果请求的URI为match_id=11&kw=黑超网吧，那么
token = md5(Mpks1$01098#AlsNj + match_id11 + 黑超网吧) 
      = md5(Mpks1$01098#AlsNjmatch_id11黑超网吧)；

如果请求的URI为match_id=11&kw=黑超网吧&netbar_id=11，那么
token = md5(Mpks1$01098#AlsNj + match_id11 + 黑超网吧 + netbar_id + 11) 
      = md5(Mpks1$01098#AlsNjmatch_id11黑超网吧netabr_id11)；
```

```
B2接口成功时返回数据：
{
	"status" : 0,
	"data" : [
		{
			"team_id" : 111, //战队id
			"team_name" : "Kiss the Rain",
			"owner" : "发起者名字",
			"owner_id" : 12, //发起者user_id
			"join_num" : 3, //当前战队已报名人数
			"limit_num" : 5, //当前战队拟定报名人数上限
			"match_id" : 1, // 赛事id
			"match_order" : 1, //场次
			"game_id" : 1, // 游戏id
			"match_order_desc" : "第一场", //场次描述
		}, 
		
		...
		
		{
			"team_id" : 2222, //战队id
			"team_name" : "Kiss the Rain N",
			"owner" : "发起者名字",
			"owner_id" : 3333, //发起者user_id
			"join_num" : 3, //当前战队已报名人数
			"limit_num" : 5, //当前战队拟定报名人数上限
			"match_id" : 1, // 赛事id
			"match_order" : 2, //场次
			"game_id" : 1, // 游戏id
			"match_order_desc" : "第二场", //场次描述
		},
	]
}

失败时返回数据：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B4 查看战队详情Api***

```
请求Api URL：http://221.228.215.97:9020/details/fightTeam/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|team_id|战队id|integer|是|战队id|
|token|认证token|string|是|必须传递token值，对参数校验，防止第三方抓取我们的数据|

```
`TOKEN生成：`

token = md5($private_key + team_id + $team_id)

如想请求team_id=1的战队详情，则 token = md5(Mpks1$01098#AlsNjteam_id1)
```

```
接口请求成功时返回数据格式：
{
	"status" : 0,
	"data" : {
		"team_name" : "战队名字",
		"team_id" : 1, // 战队id
		"match_lists" : [
			{
				"city_id" : 11, //城市id
				"city_name" : "上海", //城市名字
				"match_name" : "比赛名称1",
				"match_order" : 1, // 比赛场次 integert
				"time" : "2015-12-31", //比赛时间
				"match_address" : "比赛网吧名称"
			},
			
			... 
			
			{
				"city_id" : 11, //城市id
				"city_name" : "上海", //城市名字
				"match_name" : "比赛名称1",
				"match_order" : 2, // 比赛场次 integert
				"time" : "2015-12-31", //比赛时间
				"match_address" : "比赛网吧名称"
			}
		], // 已报赛事
		
		"team_members" : [
			{
				"icon_url"   : "http://xxxx.jpg", // 队员图像
				"user_name"  : "xxx",  // 战队成员名字
				"role_name"  : "队长", // 在战队中角色
				"advantange" : "Carry", // 擅长位置 
			},
			
			...
			
			{
				"icon_url"   : "http://xxxx.jpg", // 队员图像
				"user_name"  : "xxx",  // 战队成员名字
				"role_name"  : "队长", // 在战队中角色
				"advantange" : "Carry", // 擅长位置 
			}
		] // 队员名单
	}
}

失败时返回数据：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B5 加入战队Api***

```
请求Api URL：http://221.228.215.97:9020/activities/joinTeam/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|team_id|战队id|integer|是|战队id|
|user_id|用户id|integer|是|必须传递user_id，它是网吧用户的 user_id|
|identify_name|用户名|string|是|与身份证上的姓名完全一直|
|identify_id|身份证号|string|是|请确认身份证号输入正确|
|advantage|擅长位置|string|否|选手擅长位置|
|token|认证token|string|是|必须传递token值，对参数校验|

```
如果请求参数顺序为team_id=1&user_id=2&identify_name=小明&identify_id={$身份证号},那么

token = md5(Mpks1$01098#AlsNj + tema_id + 1+user_id 
	     		+ 2 + identify_name + 小明+identify_id + {$身份证号})
```

```
Api成功时返回数据格式如下：
{
	"status" : 0,
	"data" : "$success_msg"
}

失败时返回数据：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

- ***B6 查看战队申请列表API***

```
这个PRD上说是下一个迭代再做，这里先占个位
```

- ***B7 解散战队Api***

```
请求Api URL：http://221.228.215.97:9020/activities/dissolve

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|team_id|战队id|integer|是|战队id|
|user_id|队长的用户id|integer|是|必须传递队长的user_id，它是网吧用户的user_id|
|token|认证token|string|是|必须传递token值，对参数校验|

```
如请求参数顺序是team_id=121&user_id=22, 那么
	
TOKEN = md5(Mpks1$01098#AlsNjteam_id121user_id22)
```

```
成功时接口返回数据格式：
{
	"status" : 0,
	"data"   : {
		"msg"  : "$success_msg", // 解散战队成功后提示
		"uid"  : $user_id, // 谁解散的战队，一般为队长的user_id
		"time" : "2015-12-24 13:23:40" // 解散战队时间
	}
}

失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B8 分享战队H5 Page***
	
```
H5页面URL为：http://221.228.215.97:9020/share/fightTeam/{$fightTeamId}
	
分享不需要Token验证，页面内容由运营人员通过后台工具编辑
```
	
---	

- ***B9 获取某一赛事的特定网吧列表Api(依赖match_id)***

```
Api描述：根据某一赛事的赛事id，即match_id 获取举办该赛事网吧列表
	
Api地址：http://221.228.215.97:9020/lists/getNetbarListsByMatchId/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id，必传参数|
|token|认证token|string|是|必须传递token值，对参数校验|
	
```
TOKEN生成规则：
	
例如请求的参数是match_id=10,则
token = md5(Mpks1$01098#AlsNj + match_id + 10)
      = md5(Mpks1$01098#AlsNjmatch_id10)
```
	
```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : [
		{
			"icon_url" : "{$网吧图片地址}",
			"netbar_id" : 1, //网吧id，选中网吧时可能需要帮这个回传给服务端
			"netbar_name" : "网鱼网咖", //网吧名字
			"cost_min" : 10, //每小时最少消费金额，单位元
			"cost_max" : 15, //每小时最多消费金额，单位元
			"address" : "比赛详细地址",
		},
		
		...
		
		{
			"icon_url" : "{$网吧图片地址}",
			"netbar_id" : 1, //网吧id，选中网吧时可能需要帮这个回传给服务端
			"netbar_name" : "网鱼网咖", //网吧名字
			"cost_min" : 10, //每小时最少消费金额，单位元
			"cost_max" : 15, //每小时最多消费金额，单位元
			"address" : "比赛详细地址",
		},
	]
}
	
失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B10 获取某一赛事的比赛场次列表Api(依赖赛事match_id)***

```
Api描述：根据某一赛事的赛事id，即match_id 获取举办该赛事网吧列表
	
Api地址：http://221.228.215.97:9020/lists/getNetbarListsByMatchId/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id，必传参数|
|token|认证token|string|是|必须传递token值，对参数校验|
	
```
TOKEN生成规则：
	
例如请求的参数是match_id=10,则
token = md5(Mpks1$01098#AlsNj + match_id + 10)
      = md5(Mpks1$01098#AlsNjmatch_id10)
```
	
```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : [
		{
			"match_order" : 1, //场次id
			"match_order_desc" : "第一场", //场次中文描述
			"time" : "2015-12-12 14:00", //比赛时间
			"address" : "比赛地址",
		},
		
		...
		
		{
			"match_order" : 3, //场次id
			"match_order_desc" : "第三场", //场次中文描述
			"time" : "2015-12-15 14:00", //比赛时间
			"address" : "比赛地址",
		}
	]
}
	
失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B11 获取某一赛事的区服列表Api(依赖赛事match_id)***

```
Api描述：根据某一赛事的赛事id，即match_id 获取举办该赛事区和服列表
	
Api地址：http://221.228.215.97:9020/lists/getAreaServiceListsByMatchId/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id，必传参数|
|token|认证token|string|是|必须传递token值，对参数校验|
		
```
TOKEN生成规则：
	
例如请求的参数是match_id=10,则
token = md5(Mpks1$01098#AlsNj + match_id + 10)
      = md5(Mpks1$01098#AlsNjmatch_id10)
```
	
```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : [
		{
			"area_id" : 1, //区id
			"area_name" : "{$区名}",
			"group_id" : 2, //游戏分组id
			"group_name" : "{$游戏分组名}",
			"server_id" : 3, //服id
			"server_name" : "{$服名}",
		},
		
		...
		
		{
			"area_id" : 3, //区id
			"area_name" : "{$区名}",
			"group_id" : 4, //游戏分组id
			"group_name" : "{$游戏分组名}",
			"server_id" : 5, //服id
			"server_name" : "{$服名}",
		}
	]
}
	
失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***B12 创建战队Api***

```
Functions: 队长创建战队基本信息，写战队与赛事关联表信息，执行创建者加入这个战队动作

Api地址：http://221.228.215.97:9020/activities/createFightTeam/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|match_id|赛事id|integer|是|赛事id，必传参数|
|match_order|赛事场次id|integer|是|1：第一场 2：第二场 。。。|
|team_name|战队名字|string|是|战队名字，必传参数，最多255字符|
|logo|战队图片地址|string|否|战队logo|
|team_info|战队简介|string|否|简单描述战队的一些信息|
|identify_type|身份类型|integer|是|(暂定)1：身份证 2：护照 3：港澳台签证 4：军人证|
|user_name|报名时的名字|string|是|报名时填写的名字，不一定是用户的user_name|
|identify_id|证件号码|string|是|证件编号，字符串类型|
|mobile|手机号|string|是|必须符合手机号类型特征，在大陆11位数字组成的字符串|
|advantage|擅长角色|string|否|用户擅长的位置|
|status|报名方式|integer|是|1：线上报名 队长报名即创建战队肯定是线上报名的|
|token|认证token|string|是|必须传递token值，对参数校验|

```
创建战队成功时数据返回：
{
	"status" : 0,
	"data" : {
		"team_id" : 1, //成功创建战队的id
		"match_id" : "$match_id", // 比赛id
		"match_order" : "$match_order", //比赛场次
		"msg" : "success",
	}
}

失败返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

```
`TOKEN`生成规则：

遵循`全局约定`中token生成规则，按参数顺序来一次把key value累加，然后再在累加的字符串之前加上私钥。例如请求的参数为$key1=$value1&$key2=$value2,则

token = md5($private_key + $key1 + $value1 + $key2 + $value2)
```

---

- ***C1 个人报名赛事Api***
	
```
个人报名参赛没有角色之分，因为只有一个人

Api请求地址：http://221.228.215.97:9020/activities/personalApply/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|个人报名用户id|integer|是|报名者user_id必须由客户端传递|
|match_id|赛事id|integer|是|赛事id即match_id必须由客户端传递|
|match_order|比赛场次|interger|是|1第一场 2第二场 ...|
|identify_type|身份类型|integer|是|(暂定)1：身份证 2：护照 3：港澳台签证 4：军人证|
|user_name|报名时的名字|string|是|报名时填写的名字，不一定是用户的user_name|
|identify_id|证件号码|string|是|证件编号，字符串类型|
|mobile|手机号|string|是|必须符合手机号类型特征，在大陆11位数字组成的字符串|
|advantage|擅长角色|string|否|用户擅长的位置|
|status|报名方式|integer|是|1：线上报名 2：线下报名|
|token|认证token|string|是|必须传递token值，对参数校验|

```
`TOKEN`生成规则：

遵循`全局约定`中token生成规则，按参数顺序来一次把key value累加，然后再在累加的字符串之前加上私钥。例如请求的参数为$key1=$value1&$key2=$value2,则

token = md5($private_key + $key1 + $value1 + $key2 + $value2)
```

```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : {
		"apply_id" : 121, //报名id
		"msg" : "success",
	}
}

失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***C2 团吧报名参赛Api***

```
PS：团队报名参赛有角色之分，包括队长、队员、替补等

Api用途：团队报名参加比赛API

Api地址：http://221.228.215.97:9020/activities/teamApply/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|team_id|战队id|integer|是|战队id必须由客户端传递|
|match_id|赛事id|integer|是|赛事id即match_id必须由客户端传递|
|match_order|比赛场次|interger|是|1第一场 2第二场 ...|
|user_id|报名用户id|integer|是|报名者user_id必须由客户端传递|
|identify_type|身份类型|integer|是|(暂定)1：身份证 2：护照 3：港澳台签证 4：军人证|
|user_name|报名时的名字|string|是|报名时填写的名字，不一定是用户的user_name|
|identify_id|证件号码|string|是|证件编号，字符串类型|
|mobile|手机号|string|是|必须符合手机号类型特征，在大陆11位数字组成的字符串|
|status|报名方式|integer|是|1：线上报名 2：线下报名|
|advantage|擅长角色|string|否|用户擅长的位置|
|role_id|角色id|integer|否|1：队员 2：队长 3：替补|
|token|认证token|string|是|必须传递token值，对参数校验|

```
`TOKEN`生成规则：

遵循`全局约定`中token生成规则，按参数顺序来一次把key value累加，然后再在累加的字符串之前加上私钥。例如请求的参数为$key1=$value1&$key2=$value2,则

token = md5($private_key + $key1 + $value1 + $key2 + $value2)
```

```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : {
		"team_id" : 121, //战队id
		"match_id" : 1, //赛事id
		"match_order" : 1, //比赛场次
		"msg" : "success",
	}
}

失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***P1 根据user_id获取网吧用户信息Api***

```
Api描述：根据用户id，即user_id获取用户的信息
	
Api地址：http://221.228.215.97:9020/user/getInfo/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|user_id|用户id|integer|是|用户id，必传参数|
|token|认证token|string|是|必须传递token值，对参数校验|
	
```
TOKEN生成规则：
	
例如请求的参数是user_id=10,则
token = md5(Mpks1$01098#AlsNj + user_id + 10)
      = md5(Mpks1$01098#AlsNjuser_id10)
```
	
```
Api请求成功时数据格式：
{
	"status" : 0,
	"data"   : {
		"user_id" : 1,
		"user_name" : "Map Reduce", //用户名
		"mobile" : "13227893976", //手机号 字符串
		"identify_name" : "{$认证名字}", //如身份证、护照
		"identify_id" : "123456789123456789", //证件编号 字符串
 	}
}	

失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***P2 获取身份认证类型Api***

```
Functions：获取支持的身份认证信息列表
	
Api地址：http://221.228.215.97:9020/user/getIdentifyLists/
	
请求方式：POST
```
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|type|身份类型id|integer|否|1：身份证 2：护照 3：港澳台签证 4：军人证|
|token|认证token|string|是|必须传递token值，对参数校验|

```
TOKEN认证规则遵循全局约定Token生成规则

如果type=2非空，则

token = md5($private_key + type + $type)
      = md5(Mpks1$01098#AlsNjtype2)
      
如不传type参数，则

token = md5($private_key)
      = md5(Mpks1$01098#AlsNj)
```

```
Api请求成功时数据格式：

<1> 传递type且type = [1 | 2 | 3 | 4]时
{
	"status" : 0,
	"data"   : {
		"type_id" : $type, // 请求方传递的参数type值
		"type_name" : "$type_value", // 如身份证|护照 ...
 	}
}
<2> 不传type参数时成功返回数据格式：
{
	"status" : 0,
	"data" : [
		{
			"type_id" : 1,
			"type_name" : "身份证",
		},
		
		...
		
		{
			"type_id" : 4,
			"type_name" : "军人证",
		}
	]
}

失败时返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```

---

- ***P3 获取筛选项中所有游戏类型Api***

```
Functions：获取所有游戏类型，包括game_id，game_name

Api地址：http://221.228.215.97:9020/activities/getAllFilterGames/

请求方式：POST
```

|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|time|时间戳|integer|10位|是|必须传递时间戳，而且是10位的|
|token|认证token|string|是|必须传递token值，对参数校验|

```
token = md5(Mpks1$01098#AlsNj + time + $time)
      = md5(Mpks1$01098#AlsNjtime$time)

其中$time是时间戳的值
```

```
Api请求成功时返回值：
{
	"status" : 0,
	"data" : [
		{
			"game_id" : 1, //游戏id
			"game_name" : "dota2", // 游戏名字
		},
		
		...
		
		{
			"game_id" : 100, //游戏id
			"game_name" : "炉石传说", // 游戏名字
		},
	]
}

失败返回：
{
	"status": $error_code, // 非0错误码 
	"error_msg" : {
		"error" : "$error_msg"
		"time"  : "2015-12-23 11:11:20"
	},
	"data":{}
}
```


