# 云应用

终端用户通过云系统客户端访问**远程**云端应用程序。

## 启动云应用

用户点击云应用图标将调用该接口，向云系统发起应用启动请求.

|         		|	类型           		|
|---------------|-----------------------|
|接口类型 		|	Http Restful API |
|Http请求类型	|	POST	|
|Body参数		|	JSON	|

### Body 参数
|       字段  	|	类型     |       描述  				|
|---------------|-----------|---------------------------|
|category		|	整形 	| 应用类别 0:一般应用，1:游戏 	|
|appName		|	字符串	| 应用名称 eg：WebStorm 		|
|appParams		|	字符串	| 应用启动参数			 	|
|userToken		|	字符串	| 用户令牌					|
|device			|	JSON	| 用户终端设备信息				|

*** device设备信息 JSON 定义 ***

|    字段  	|	类型		|       描述  				
|-----------|-----------|-------------------------------------------------
|category	|	整形 	| 设备类别 0:pc/laptop，1:手机，2:平板 	
|platform	|	整形		| 设备操作系统名称 0:windows，1:macOS， 2:android，3:ios 	
|version	|	字符串	| 操作系统版本 eg：12.1.4			 	
|model		|	字符串	| 设备型号				
|cpu		|	字符串	| cpu信息				
|memory		|	字符串	| 内存信息				
|screen		|	字符串	| 屏幕大小及分辨率信息		

*** Body参数JSON示例 ***
```json
{
	"category": 0,
	"appName": "WebStorm",
	"appParams": "",
	"userToken": "sdfsadfsdfsdfsdfsdfdsfsdfsdf",
    "device": {
        "category": 0,
        "platform": 0,
        "version": "win10",
        "model": "DELL08976",
        "cpu": "i7-8700@3.20G",
        "memory":"8GB",
        "screen": "1920*1080"
    }
}
```
. 启动成功
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 334

{
  "accessTime" : "2019-03-04T14:47:13.984+08:00",
  "duration" : "5s"
}
```
