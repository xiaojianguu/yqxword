# JSON风格指南 #

版本：[1.0]()

## 简介 ##

该风格指南是对在Google创建JSON APIs而提供的指导性准则和建议。总体来讲，JSON APIs应遵循JSON.org上的规范。这份风格指南澄清和标准化了特定情况，从而使Google的JSON APIs有一种标准的外观和感觉。这些指南适用于基于RPC和基于REST风格的API的JSON请求和响应。

## 定义 ##
为了更好地实现这份风格指南的目的，下面几项需要说明：

* 属性(property) - JSON对象内的键值对(name/value pair)
* 属性名(property name) - 属性的名称(或键)
* 属性值(property value) - 分配给属性的值

示例：

	{
	  // 一组键值对称作一个 "属性".
	  "propertyName": "propertyValue"
	}
	
	
Javascript的数字(*number*)包含所有的浮点数,这是一个宽泛的指定。在这份指南中，数字(*number*)指代Javascript中的数字(*number*)类型，而整型(*integer*)则指代整型。

## 一般准则 ##
### 注释 ###
**JSON对象中不包含注释。**

JSON对象中不应该包含注释。该指南中的某些示例含有注释。但这仅仅是为了说明示例。

	{
	  // 你可能在下面的示例中看到注释,
	  // 但不要在你的JSON数据中加入注释.
	  "propertyName": "propertyValue"
	}

### 双引号 ###
**使用双引号**

如果（某个）属性需要引号，则必须使用双引号。所有的属性名必须在双引号内。字符类型的属性值必须使用双引号。其它类型值（如布尔或数字）不应该使用双引号。

### 扁平化数据 VS 结构层次 ###
**不能为了方便而将数据任意分组**

JSON中的数据元素应以*扁平化*方式呈现。不能为了方便而将数据任意分组。

在某些情况下，比如描述单一结构的一批属性，因为它被用来保持结构层次，因而是有意义的。但是遇到这些情况还是应当慎重考虑，记住只有语义上有意义的时候才使用它。例如，一个地址可以有表示两种方式，但结构化的方式对开发人员来讲可能更有意义：

扁平化地址: 单一结构的一批属性

	{
	  "company": "Google",
	  "website": "http://www.google.com/",
	  "addressLine1": "111 8th Ave",
	  "addressLine2": "4th Floor",
	  "state": "NY",
	  "city": "New York",
	  "zip": "10011"
	}
	
结构化地址：语义上有意义

	{
	  "company": "Google",
	  "website": "http://www.google.com/",
	  "address": {
	    "line1": "111 8th Ave",
	    "line2": "4th Floor",
	    "state": "NY",
	    "city": "New York",
	    "zip": "10011"
	  }
	}
	
## 属性名准则 ##

### 属性名格式 ###
**选择有意义的属性名**

属性名必须遵循以下准则:

* 属性名应该是具有定义语义的有意义的名称。
* 属性名必须是驼峰式的，ASCII码字符串。
* 首字符必须是字母，下划线(*_*)或美元符号(*$*)。
* 随后的其他字符可以是字母，数字，下划线(*_*)或美元符号(*$*)。
* 应该避免使用Javascript中的保留关键字(下文附有Javascript保留字清单)

这些准则反映JavaScript标识符命名的指导方针。使JavaScript的客户端可以使用点符号来访问属性。(例如, `result.thisIsAnInstanceVariable`). 

下面是一个对象的一个属性的例子：

	{
	  "thisPropertyIsAnIdentifier": "identifier value"
	}
	
### JSON Map中的键名 ###
**在JSON Map中键名可以使用任意Unicode字符**

当JSON对象作为Map(映射)使用时，属性的名称命名规则并不适用。Map（也称作关联数组）是一个具有任意键/值对的数据类型，这些键/值对通过特定的键来访问相应的值。JSON对象和JSON Map在运行时看起来是一样的；这个特性与API设计相关。当JSON对象被当作map使用时，API文件应当做出说明。

Map的键名不一定要遵循属性名称的命名准则。键名可以包含任意的Unicode字符。客户端可使用maps熟悉的方括号来访问这些属性。（例如`result.thumbnails["72"]`）

	{
	  // "address" 属性是一个子对象
	  // 包含地址的各部分.
	  "address": {
	    "addressLine1": "123 Anystreet",
	    "city": "Anytown",
	    "state": "XX",
	    "zip": "00000"
	  },
	  // "address" 是一个映射
	  // 含有响应规格所对应的URL，用来映射thumbnail url的像素规格
	  "thumbnails": {
	    "72": "http://url.to.72px.thumbnail",
	    "144": "http://url.to.144px.thumbnail"
	  }
	}
	
### 保留的属性名称 ###
**某些属性名称会被保留以便能在多个服务间相容使用**

保留属性名称的详细信息，连同完整的列表，可在本指南后面的内容中找到。服务应按照被定义的语义来使用属性名称。

### 单数属性名 VS 复数属性名 ###
**数组类型应该是复数属性名。其它属性名都应该是单数。**

数组通常包含多个条目，复数属性名就反映了这点。在下面这个保留名称中可以看到例子。属性名*items*是复数因为它描述的是一组对象。大多数的其它字段是单数。

当然也有例外，尤其是涉及到数字的属性值的时候。例如，在保留属性名中，*totalItems* 比 *totalItem*更合理。然后，从技术上讲，这并不违反风格指南，因为 *totalItems* 可以被看作 *totalOfItems*, 其中 *total* 是单数（依照风格指南），*OfItems* 用来限定总数。字段名也可被改为 *itemCount*，这样看起来更象单数.

	{
	  // 单数
	  "author": "lisa",
	  // 一组同胞, 复数
	  "siblings": [ "bart", "maggie"],
	  // "totalItem" 看起来并不对
	  "totalItems": 10,
	  // 但 "itemCount" 要好些
	  "itemCount": 10
	}

### 命名冲突 ###
**通过选择新的属性名或将API版本化来避免命名冲突**

新的属性可在将来被添加进保留列表中。JSON中不存在命名空间。如果存在命名冲突，可通过选择新的属性名来解决这个问题。例如，假设我们由下面的JSON对象开始：

	{
	  "apiVersion": "1.0",
	  "data": {
	    "recipeName": "pizza",
	    "ingredients": ["tomatoes", "cheese", "sausage"]
	  }
	}
	
如果我们希望将来把*ingredients*列为保留字，我们可以通过下面来达成:

1.选一个不同的名字

	{
	  "apiVersion": "1.0",
	  "data": {
	    "recipeName": "pizza",
	    "ingredientsData": "Some new property",
	    "ingredients": ["tomatoes", "cheese", "sausage"]
	  }
	}

	
## 属性值准则 ##
### 属性值格式 ###
**属性值必须是Unicode 的 booleans（布尔）, 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays), 或 null.**

JSON.org上的标准准确地说明了哪些类型的数据可以作为属性值。这包含Unicode的布尔(booleans), 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays), 或 null。JavaScript表达式是不被接受的。APIs应该支持该准则，并为某个特定的属性选择最合适的数据类型（比如，用numbers代表numbers等）。

例子：

	{
	  "canPigsFly": null,     // null
	  "areWeThereYet": false, // boolean
	  "answerToLife": 42,     // number
	  "name": "Bart",         // string
	  "moreData": {},         // object
	  "things": []            // array
	}
	
### 空或Null 属性值 ###
**考虑移除空或null值**

如果一个属性是可选的或者包含空值或*null*值，考虑从JSON中去掉该属性，除非它的存在有很强的语义原因。

	{
	  "volume": 10,
	
	  // 即使 "balance" 属性值是零, 它也应当被保留,
	  // 因为 "0" 表示 "均衡" 
	  // "-1" 表示左倾斜和"＋1" 表示右倾斜
	  "balance": 0,
	
	  // "currentlyPlaying" 是null的时候可被移除
	  // "currentlyPlaying": null
	}
	
### 枚举值 ###
**枚举值应当以字符串的形式呈现**

随着APIs的发展，枚举值可能被添加，移除或者改变。将枚举值当作字符串可以使下游用户优雅地处理枚举值的变更。

Java代码：

	public enum Color {
	  WHITE,
	  BLACK,
	  RED,
	  YELLOW,
	  BLUE
	}
	
JSON对象：

	{
	  "color": "WHITE"
	}
	
## 属性值数据类型 ##

上面提到，属性值必须是布尔(booleans), 数字(numbers), 字符串(strings), 对象(objects), 数组(arrays), 或 null. 然而在处理某些值时，定义一组标准的数据类型是非常有用的。这些数据类型必须始终是字符串，但是为了便于解析，它们也会以特定的方式被格式化。

### 日期属性值 ###
**日期应该使用RFC3339建议的格式**

日期应该是RFC 3339所建议的字符串格式。

	{
	  "lastUpdate": "2007-11-06T16:34:41.000Z"
	}
	
### 时间间隔属性值 ###
**时间间隔应该使用ISO 8601建议的格式**

时间间隔应该是ISO 8601所建议的字符串格式。

	{
	  // 三年, 6个月, 4天, 12小时,
	  // 三十分钟, 5秒
	  "duration": "P3Y6M4DT12H30M5S"
	}
	
### 纬度/经度属性值 ###
**纬度/经度应该使用ISO 6709建议的格式**

纬度/经度应该是ISO 6709所建议的字符串格式。
而且, 它应该更偏好使用 e Â±DD.DDDDÂ±DDD.DDDD 角度格式.

	{
	  // 自由女神像的纬度/经度位置.
	  "statueOfLiberty": "+40.6894-074.0447"
	}
	
## JSON结构和保留属性名 ##

为了使APIs保持一致的接口，JSON对象应当使用以下的结构。该结构适用于JSON的请求和响应。在这个结构中，某些属性名将被保留用作特殊用途。这些属性并不是必需的，也就是说，每个保留的属性可能出现零次或一次。但是如果服务需要这些属性，建议遵循该命名条约。下面是一份JSON结构语义表，以Orderly格式呈现(现在已经被纳入 JSONSchema)。你可以在该指南的最后找到关于JSON结构的例子。

	object {
	  string apiVersion?;
	  string context?;
	  string id?;
	  string method?;
	  object {
	    string id?
	  }* params?;
	  object {
	    string kind?;
	    string fields?;
	    string etag?;
	    string id?;
	    string lang?;
	    string updated?; # date formatted RFC 3339
	    boolean deleted?;
	    integer currentItemCount?;
	    integer itemsPerPage?;
	    integer startIndex?;
	    integer totalItems?;
	    integer pageIndex?;
	    integer totalPages?;
	    string pageLinkTemplate /^https?:/ ?;
	    object {}* next?;
	    string nextLink?;
	    object {}* previous?;
	    string previousLink?;
	    object {}* self?;
	    string selfLink?;
	    object {}* edit?;
	    string editLink?;
	    array [
	      object {}*;
	    ] items?;
	  }* data?;
	  object {
	    integer code?;
	    string message?;
	    array [
	      object {
	        string domain?;
	        string reason?;
	        string message?;
	        string location?;
	        string locationType?;
	        string extendedHelp?;
	        string sendReport?;
	      }*;
	    ] errors?;
	  }* error?;
	}*;
	
JSON对象有一些顶级属性，然后是*data*对象或*error*对象，这两者不会同时出现。下面是这些属性的解释。

## 顶级保留属性名称 ##
**顶级的JSON对象可能包含下面这些属性**

### apiVersion ###

	属性值类型: 字符串(string)
	父节点: -

呈现请求中服务API期望的版本，以及在响应中保存的服务API版本。应随时提供*apiVersion*。这与数据的版本无关。将数据版本化应该通过其他的机制来处理，如etag。

示例：

	{ "apiVersion": "2.1" }

### data ###

	属性值类型: 对象(object)
	父节点: -
	
示例：

	{
	  "apiVersion": "2.0",
	  "data": {
	    "message": "File Found",
		{
	      "domain": "Calendar",
	      "message": "File Found
	    }
	  }
	}

*现有做法:*
	
	{
	  "resultCode": "00100000"
	  "resultMsg": "success",
	  "content": {
	    "message": "File Found",
		{
	      "domain": "Calendar",
	      "message": "File Found"
	    }
	  },
	  "accessToken":null
	}	
	
包含响应的所有数据。该属性本身拥有许多保留属性名，下面会有相应的说明。服务可以自由地将自己的数据添加到这个对象。一个JSON响应要么应当包含一个*data*对象，要么应当包含*error*对象，但不能两者都包含。如果*data*和*error*同时出现，则*error*对象优先。

### error ###

	属性值类型: 对象(object)
	父节点: -
	
表明错误发生，提供错误的详细信息。错误的格式支持从服务返回一个或多个错误。一个JSON响应可以有一个*data*对象或者一个*error*对象，但不能两者都包含。如果*data*和*error*都出现，*error*对象优先。

示例：

	{
	  "apiVersion": "2.0",
	  "error": {
	    "code": 404,
	    "message": "File Not Found",
	    "errors": [{
	      "domain": "Calendar",
	      "reason": "ResourceNotFoundException",
	      "message": "File Not Found
	    }]
	  }
	}

*现有做法:*
	
	{
	  "resultCode": "00100001"
	  "resultMsg": "页面存在错误",
	  "content": null,
	  "accessToken":null
	}

## 用于分页的保留属性名 ##

下面的属性位于*data*对象中，用来给一列数据分页。一些语言和概念是从OpenSearch规范中借鉴过来的。

下面的分页数据风格的分页，包括：


	{
	  "resultCode" : "00100000",
	  "resultMsg" : "success",
	  "content" : {
	    "pageNo" : 1,
	    "pageSize" : 5,
	    "totalElements" : 42,
	    "totalPages" : 9,
	    "items":[
	    
	    ]
		},
		"accessToken":null
	}

### data.pageIndex ###

*现有做法* data.pageNo

	属性值类型: 整数(integer)
	父节点: data

结果集中当前的页数

### data.pageSize ###

	属性值类型: 整数(integer)
	父节点: data
结果集中一页最多放的结果数目

### data.totalItems ###
*现有做法* data.totalElements

	属性值类型: 整数(integer)
	父节点: data
结果集中总共的结果集数目

### data.totalPages ###


	属性值类型: 整数(integer)
	父节点: data
	
结果集中当前的总页数

##错误对象中的保留属性名##
JSON对象的*error*属性应包含以下属性。

### error.code ###

	属性值类型: 整数(integer)
	父节点: error

表示该错误的编号。这个属性通常表示HTTP响应码。如果存在多个错误，*code*应为第一个出错的错误码。

示例：

	{
	  "error":{
	    "code": 404
	  }
	}

### error.message ###

	属性值类型: 字符串(string)
	父节点: error

一个人类可读的信息，提供有关错误的详细信息。如果存在多个错误，*message*应为第一个错误的错误信息。

示例：

	{
	  "error":{
	    "message": "File Not Found"
	  }
	}	

### error.errors ###

	属性值类型: 数组(array)
	父节点: error
	
包含关于错误的附加信息。如果服务返回多个错误。*errors*数组中的每个元素表示一个不同的错误。
	
示例：

	{ "error": { "errors": [] } }	
	
### error.errors[].domain ###

	属性值类型: 字符串(string)
	父节点: error.errors
	
服务抛出该错误的唯一识别符。它帮助区分服务的从普通协议错误(如,找不到文件)中区分出具体错误(例如，给日历插入事件的错误)。

示例：

	{
	  "error":{
	    "errors": [{"domain": "Calendar"}]
	  }
	}
	
### error.errors[].reason ###

	属性值类型: 字符串(string)
	父节点: error.errors

该错误的唯一识别符。不同于*error.code*属性，它不是HTTP响应码。

示例：

	{
	  "error":{
	    "errors": [{"reason": "ResourceNotFoundException"}]
	  }
	}

### error.errors[].message ###

	属性值类型: 字符串(string)
	父节点: error.errors
	
一个人类可读的信息，提供有关错误的更多细节。如果只有一个错误，该字段应该与*error.message*匹配。

示例：

	{
	  "error":{
	    "code": 404
	    "message": "File Not Found",
	    "errors": [{"message": "File Not Found"}]
	  }
	}		


## 示例 ##
### YouTube JSON API ###

这是YouTube JSON API响应对象的示例。你可以从中学到更多关于YouTube JSON API的内容：[http://code.google.com/apis/youtube/2.0/developers_guide_jsonc.html](http://code.google.com/apis/youtube/2.0/developers_guide_jsonc.html)

	{
	  "apiVersion": "2.0",
	  "data": {
	    "updated": "2010-02-04T19:29:54.001Z",
	    "totalItems": 6741,
	    "startIndex": 1,
	    "itemsPerPage": 1,
	    "items": [
	      {
	        "id": "BGODurRfVv4",
	        "uploaded": "2009-11-17T20:10:06.000Z",
	        "updated": "2010-02-04T06:25:57.000Z",
	        "uploader": "docchat",
	        "category": "Animals",
	        "title": "From service dog to SURFice dog",
	        "description": "Surf dog Ricochets inspirational video ...",
	        "tags": [
	          "Surf dog",
	          "dog surfing",
	          "dog",
	          "golden retriever",
	        ],
	        "thumbnail": {
	          "default": "http://i.ytimg.com/vi/BGODurRfVv4/default.jpg",
	          "hqDefault": "http://i.ytimg.com/vi/BGODurRfVv4/hqdefault.jpg"
	        },
	        "player": {
	          "default": "http://www.youtube.com/watch?v=BGODurRfVv4&feature=youtube_gdata",
	          "mobile": "http://m.youtube.com/details?v=BGODurRfVv4"
	        },
	        "content": {
	          "1": "rtsp://v5.cache6.c.youtube.com/CiILENy73wIaGQn-Vl-0uoNjBBMYDSANFEgGUgZ2aWRlb3MM/0/0/0/video.3gp",
	          "5": "http://www.youtube.com/v/BGODurRfVv4?f=videos&app=youtube_gdata",
	          "6": "rtsp://v7.cache7.c.youtube.com/CiILENy73wIaGQn-Vl-0uoNjBBMYESARFEgGUgZ2aWRlb3MM/0/0/0/video.3gp"
	        },
	        "duration": 315,
	        "rating": 4.96,
	        "ratingCount": 2043,
	        "viewCount": 1781691,
	        "favoriteCount": 3363,
	        "commentCount": 1007,
	        "commentsAllowed": true
	      }
	    ]
	  }
	}
	
	
## Api 具体规范

| 名称 | 描述 |
|---- |----|
|字段相同含义值一致性| 如产品类型、订单类型|
|JSON字段|多余不需要的字段不返回(@JsonIgnore) |
|JSON字段|有意义的字段名 |
|集合值|以JsonArray 方式返回|
|标签| 尽量不要使用标签文本(富文本)|
|要求有顺序的Map| 用数组传递|


-_EOF_-