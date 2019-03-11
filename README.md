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

