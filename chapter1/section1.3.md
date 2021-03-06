Axletree项目的路由，以及页面对应的api接口，采用的都是可配置形式。

#配置
每一个axletree项目(例如axledemo)都会在client目录下生成一个配置文件apiConf.js，其内容大体如下:

```
module.exports = {
	"axledemo": {
		"pages": [
			{
				"page": "index",
				"urls": [],
				"cache": true,
				"persist": false
			}
		]
	}
}
```
其中，"page"对应着路由和页面。开发模式下，路由被定义为

```
#项目名 + 页面名
/axledemo/index
```
正式环境中，路由还跟cache参数有关，cache配置页面是否需要在node层缓存请求的api接口数据。当cache设置为true时，路由跟上面的相同，当设置为false时，路由被定义为

```
#live + 项目名 + 页面名
/live/axledemo/index

```

apiConf.js中的urls配置的是api数据接口，其形式如下:

```
[
	{
		"name": "",
		"url":""
	}
]
```
其中，"name"指的是api名称，在模版中将能引用请求的结果。


#缓存系统
axletree能够根据配置决定是否缓存api接口，当缓存了数据时，缓存的有效期默认是60s，采用这种缓存系统带来了诸多好处:

- 极大的提升了系统的性能和体验。
- 提高了整个系统的容错性和健壮性。

针对第二点说明下，由于在请求api接口时不可避免会因为网络或者其它不可控因素导致访问失败、出错、返回502等，这时候假如直接返回一个错误页将大大降低体验，为此，axletree在出现这些情况时会忽略缓存有效期，直接使用缓存系统的数据渲染模版返回。


#持久化
有些项目在经过一定的时间运行后，会有持久化的需求，即将后台数据本地化,使项目不在依赖于接口运行。
在Axletree中要实现这个功能很简单，只需要在需要持久化的阶段，将配置中的"persist"参数设置为true即可(注:非持久化时期必须设置为false)
