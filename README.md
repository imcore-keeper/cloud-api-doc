# 应用云管理

终端用户通过云系统客户端访问**远程**云端应用程序。
应用云管理包含应用云管理服务器和应用agent两部分。
>应用云管理服务器负责解释云平台发送来的应用请求消息体（json格式），
>然后将解释后的消息，封装成应用agent能够识别的指令，发送给相应的agent。

>Agent接收到消息指令后，执行指令以打开或者关闭应用。

## 1. 启动云应用

用户点击云应用图标将调用该接口，向云系统发起应用启动请求.

### 1.1 Request Body 参数
|       字段  	|	类型     |       描述  				|
|---------------|-----------|---------------------------|
|category		|	整形 	| 应用类别 0:一般应用，1:游戏 	|
|appName		|	字符串	| 应用名称 eg：WebStorm 		|
|appParams		|	字符串	| 应用启动参数			 	|				|
|device			|	JSON	| 用户终端设备信息				|


*** device设备信息 JSON 定义 ***

|       字段  	|	类型     |       描述  				|
|---------------|-----------|---------------------------|
|category		|	整形 	| 设备类别 0:台式机/笔记本，1:手机，2:平板	|
|platform		|	字符串	| 设备操作系统名称 0:windows，1:macOS， 2:android，3:ios 		|
|version		|	字符串	| 操作系统版本 eg：12.1.4			 	|
|model		|	字符串	| 设备型号					|
|memory			|	字符串	| 内存信息				|
|screen			|	字符串	| 屏幕分辨率信息				|	


*** Body参数JSON示例 ***
```json
{
	"category": 0,
	"appName": "WebStorm",
	"appParams": "",
    	"device": {
        	"category": 0,
        	"platform": 0,
        	"version": "win10",
        	"model": "DELL08976",
        	"screen": "1920*1080"
    	}
}
```
### 1.2 Response Body Field类型说明
|    字段  	|	类型		|       描述  				
|-----------|-----------|-------------------------------------------------
|accessTime	|	字符串	| 调用时间戳	
|duration	|	字符串	| 调用花费时长，单位ms	
|error	|	JSON	| 错误消息体，应用启动成功，该字段为空		 	

#### Http Response 示例
- 启动成功
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 334

{
  "accessTime" : "2019-03-04T14:47:13.984+08:00",
  "duration" : "5s"
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
  "error" : {
	  "code" : 5001,
	  "message" : "no available resources"
  }
}
```
#### Response Error Code 说明
. 5001
> 云资源繁忙，没有可用云资源VM可以被调度

. 5002
> 有可用云资源，但是云资源VM调度失败

. 5003
> 云资源VM调度成功，目标应用启动失败

. 4001
> 用户令牌错误：令牌失效，令牌非法

. 4002
> 用户没有足够权利调用目标应用，原因：用户没有购买目标应用服务等

. 4003
> 目前不支持该用户设备，原因：操作系统等
