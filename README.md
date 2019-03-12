# 应用云管理

终端用户通过云系统客户端访问**远程**云端应用程序。
应用云管理包含应用云管理服务器和应用agent两部分。

**应用云管理服务器**

>解释云平台发送来的应用请求消息体（json格式），
>然后将解释后的消息，封装成应用agent能够识别的指令，发送给相应的agent。

**应用Agent**

>Agent接收到消息指令后，执行指令以打开或者关闭应用，并将执行结果通知应用云管理服务器

## 1. 云应用启动消息定义

用户点击云应用图标，向云平台发起应用启动请求，云平台按照该消息定义格式，封装启动消息，通过云应用服务器相应rest接口，发送至应用云管理服务器。

### 1.1 Request Body 参数
|       字段  	|	类型     |       描述  				|
|---------------|-----------|---------------------------|
|category		|	整形 	| 应用类别 0:一般应用，1:游戏 	|
|appName		|	字符串	| 应用名称 eg：WebStorm 		|
|appParams		|	字符串	| 应用启动参数(可选) 		|
|operation		|	整形 	| 应用操作码 0:关闭，1:启动	|
|device			|	JSON	| 用户终端设备信息				|


*** device设备信息 JSON 定义 ***

|       字段  	|	类型     |       描述  				|
|---------------|-----------|---------------------------|
|category		|	整形 	| 设备类别 0:台式机/笔记本，1:手机，2:平板	|
|platform		|	字符串	| 设备操作系统名称 0:windows，1:macOS， 2:android，3:ios 		|
|version		|	字符串	| 操作系统版本 eg：12.1.4			 	|
|model		|	字符串	| 设备型号					|
|screen			|	字符串	| 屏幕分辨率信息				|	


*** Body参数JSON示例 ***
```json
{
	"category": 0,
	"appName": "WebStorm",
	"appParams": "",
	"operation": 1,
    	"device": {
        	"category": 0,
        	"platform": 0,
        	"version": "win10",
        	"model": "DELL08976",
        	"screen": "1920*1080"
    	}
}
```

应用云管理服务器接收到该JSON消息后，对该消息解释并封装成agent能够识别的指令，发送给相应的应用Agent，Agent接收指令，并在目标主机上启动相应的应用，最后将应用启动结果封装成云管理服务器能够识别的指令，发送回应用云管理服务器；应用云管理服务器将该指令解释后封装成如下的JSON格式的消息，返回给平台。

### 1.2 Response Body Field类型说明
|    字段  	|	类型		|       描述  				
|-----------|-----------|-------------------------------------------------
|accessTime	|	字符串	| 调用时间戳	
|duration	|	字符串	| 调用花费时长，单位ms	
|targetAgent	|	字符串	| 目标主机IP地址
|error	|	JSON	| 错误消息体，应用启动成功，该字段为空		 	

#### Http Response 示例
- 启动成功
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 334

{
  "accessTime" : "2019-03-04T14:47:13.984+08:00",
  "duration" : "5s",
  "targetAgent" : "172.18.30.12"
}
```
- 启动失败
```http
HTTP/1.1 500 Failed
Content-Type: application/json
Content-Length: 334

{
  "accessTime" : "2019-03-04T14:47:13.984+08:00",
  "duration" : "5s",
  "targetAgent" : "172.18.30.12",
  "error" : {
	  "code" : 5001,
	  "message" : "no available resources"
  }
}
```
#### Response Error Code 说明
. 5001
> 云资源繁忙，没有可用空闲Agent主机

. 5002
> 有可用空闲Agent主机，但是应用启动失败


. 4001
> 不支持该用户设备，原因：操作系统等

. 4004
> 目标应用不存在
