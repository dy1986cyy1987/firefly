## ***firefly API开发计划***

```
一期开发【赛事、约战】活动API

+ 所有接口返回格式全局约定:

{
	"status": "ok",
	"data" : {}
}

请求成功时status是ok，其他不是ok的均请求失败
```

### ***1. 赛事API***

```
赛事API由赛事信息列表页API、赛事报道列表页API、赛事详情API、赛事分享API、战队列表页API
```

***`赛事信息列表页API`***

```
请求URL: http://www.ccjpp.xyz/lists/getMatchInfoLists/
请求方式：POST 或者 GET
```  
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|page|分页第几页|integer|否|默认1|
|rows|每页取多少条数据|integer|否|默认20|
|status|赛事状态|interger|否|1：报名中 2：报名已截止 3：赛事已结束 默认1|
|game_id|游戏类型id|integer|否|检索条件中的游戏类型id| 
|lat|经度|float|否|地理位置经度|
|lng|经度|float|否|地理位置纬度|
|area_name|区|string|否|按区域检索|

```
接口返回数据类型：
{
	"status":"ok",
	"data":[
		{
			"match_id":1111, // 赛事id
			"match_name":"Dota2世界冠军争夺赛第二场", // 赛事名称
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事图片
			"game_order":1, //场次
			"address":"上海", // 举办地址
			"start_time":"2015-12-11", // 开始时间
			"end_time":"2015-12-20",   // 结束时间
			"status":"赛事已结束",       // 赛事状态
			"status_id":3, // 赛事状态id,与status对应
			"publish_time":"2015-12-10", // 赛事发布时间
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		}, 
		{
			"match_id":4444,
			"match_name":"LOL世界冠军争夺赛第二场",
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事图片
			"game_order":1, //场次
			"address":"上海", // 举办地址
			"start_time":"2015-12-11", // 开始时间
			"end_time":"2015-12-20",   // 结束时间
			"status":"赛事已结束",       // 赛事状态
			"status_id":3, // 赛事状态id,与status对应
			"publish_time":"2015-12-10", // 赛事发布时间
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		},
		
		...
		
		{
			"match_id":2222,
			"match_name":"DOTA世界冠军争夺赛第二场",
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事图片
			"game_order":1, //场次
			"address":"上海", // 举办地址
			"start_time":"2015-12-11", // 开始时间
			"end_time":"2015-12-20",   // 结束时间
			"status":"赛事已结束",       // 赛事状态
			"status_id":3, // 赛事状态id,与status对应
			"publish_time":"2015-12-10", // 赛事发布时间
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		},
	]
}
```

----

***`赛事报道列表页API`***

```
请求URL: http://www.ccjpp.xyz/lists/getMatchReportLists/
请求方式：POST 或者 GET
```  
	
|接口参数|参数含义|数据类型|是否必传参数|附加说明|
|---    |---    |---   |---       |---    |
|page|分页第几页|integer|否|默认1|
|rows|每页取多少条数据|integer|否|默认20|
|status|赛事状态|interger|否|1：报名中 2：报名已截止 3：赛事已结束 默认1|
|game_id|游戏类型id|integer|否|检索条件中的游戏类型id| 

```
接口返回数据类型：
{
	"status":"ok",
	"data":[
		{
			"match_report_id":1111, // 赛事报道id
			"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事报道图片
			"time":"2015-12-11 14:22", // 开始时间
			"view_count":1001, // 多少人看过
			"recomment_count":54, // 评论数
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		}, 
		{
			"match_report_id":2222, // 赛事报道id
			"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事报道图片
			"time":"2015-12-11 14:22", // 开始时间
			"view_count":1001, // 多少人看过
			"recomment_count":54, // 评论数
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		}, 
				
		...
		
		{
			"match_report_id":3333, // 赛事报道id
			"match__report_name":"Dota2世界冠军争夺赛第二场", // 赛事报道名称
			"img_urls":[
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
				http://img.bianfeng.com/a/hello/mmm_12332113.jpg?mktime=221111,
			], // 赛事报道图片
			"time":"2015-12-11 14:22", // 开始时间
			"view_count":1001, // 多少人看过
			"recomment_count":54, // 评论数
			"page_info":{
				"page":1,
				"total":1001,
				"rows":20,
				"find":20
			} // 分页信息
		}, 
	]
}
```

	



