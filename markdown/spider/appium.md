## appium

#### 1.参考网址

<https://www.jianshu.com/p/7df814557c96>

#### 2.启动参数

```cmd
软件详情查询 apk包目录 > aapt dump badging 包名
	aapt dump badging weixin706android1460.apk
```

```json
	//Demo
	{
        "platformName": "Android",//模式器平台
        "deviceName": "127.0.0.1:7555",//模拟器地址端口
        "platformVersion": "8.1.0",//模拟器系统版本
        "appPackage": "com.tencent.mobileqq",//软件包名
        "appActivity": "com.tencent.mobileqq.activity.SplashActivity",//启动页  
	}	

	//qq
    {   
        "platformName": "Android",
        "deviceName": "127.0.0.1:7555",
        "platformVersion": "6.0.1",
        "appPackage": "com.tencent.mobileqq",
        "appActivity": "com.tencent.mobileqq.activity.SplashActivity",
    }
    //微信
    {
        "platformName": "Android",
        "deviceName": "127.0.0.1：62001",
        "platformVersion": "6.0.1",
        "appPackage": "com.tencent.mm",
        "appActivity": "ccom.tencent.mm.ui.LauncherUI",
        "noReset": true,
        "unicodeKeyboard": true
  }

```



#### 3.实例

