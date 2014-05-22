
已经集成的SDK
-------------
目前SDK中集成了一下6个第三方SDK

* Tapjoy
* Facebook
* Chartboost
* Google Analytics
* hasoffers
* FIKSU

之后会逐渐添加更多的第三方SDK。

在应用中引入lib工程
-------------------
请参照快速集成中（引入337lib工程）

开始集成	
--------

#. 在OnCreate方法中调用 ::

	SdkCore.initAll(Activity);

#. 在onStart方法中调用 ::

	SdkCore.onStart(Activity);

#. 在onStop方法中调用 ::

	SdkCore.onStop(Activity);

#. 在onDestroy方法中调用 ::

	SdkCore.onDestroy(Activity);

#. 在onBackPressd调用  ::

	SdkCore.onBackPressed();
	
 **3-6参数一般使用this来传递当前activity即可**
 
集成Tapjoy
----------

添加Activitys声明: ::

	<activity
	android:name="com.tapjoy.TJCOffersWebView"
	android:configChanges="orientation|keyboardHidden|screenSize" />
	<activity
	android:name="com.tapjoy.TapjoyFullScreenAdWebView"
	android:configChanges="orientation|keyboardHidden|screenSize" />
	<activity
	android:name="com.tapjoy.TapjoyDailyRewardAdWebView"
	android:configChanges="orientation|keyboardHidden|screenSize" />
	<activity
	android:name="com.tapjoy.TapjoyVideoView"
	android:configChanges="orientation|keyboardHidden|screenSize" />
	<activity
	android:name="com.tapjoy.TJAdUnitView"
	android:configChanges="orientation|keyboardHidden|screenSize"
	android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen"
	android:hardwareAccelerated="true" />
	<activity
	android:name="com.tapjoy.mraid.view.ActionHandler"
	android:configChanges="orientation|keyboardHidden|screenSize" />
	<activity
	android:name="com.tapjoy.mraid.view.Browser"
	android:configChanges="orientation|keyboardHidden|screenSize" /> 

添加meta-data声明: ::

	<meta-data android:value="请填写您获得的Tapjoy的AppID" 
	android:name="337_TAPJOY_APPID"></meta-data>
	<meta-data android:value="请填写您获得的Tapjoy的Key" 
	android:name="337_TAPJOY_APPKEY"></meta-data>
	
引入 **tapjoyconnectlibrary.jar** 文件

集成Facebook
------------

添加meta-data声明: ::

	<meta-data android:value="请填写您获得的Facebook的AppID" 
	android:name="337_FACEBOOK_APPID"></meta-data>
	
引入 **FacebookSDK lib** 工程

集成Chartboost
--------------

添加meta-data声明: ::

	<meta-data android:value="请填写您获得的Chartboost的AppID" 
	android:name="337_CHARTBOOST_APPID"></meta-data>
	<meta-data android:value="请填写您获得的Charboost的Key" 
	android:name="337_CHARTBOOST_APPKEY"></meta-data>
	
引入 **chartboost.jar**

集成Google Analytics
--------------------

在工程中/res/values/目录下，新建一个analytics.xml文件，内容如下: ::

	<?xml version="1.0" encoding="utf-8"?>
	<resources xmlns:tools="http://schemas.android.com/tools" 
	tools:ignore="TypographyDashes">
		<!--Replace placeholder ID with your tracking ID-->
		<string name="ga_trackingId">请填写Google统计的ID</string>
		<!--Enable automatic activity tracking-->
		<bool name="ga_autoActivityTracking">true</bool>
		<!--Enable automatic exception tracking-->
		<bool name="ga_reportUncaughtExceptions">true</bool>
		<bool name="ga_debug">true</bool>
	</resources>
	
引入 **libGoogleAnalyticsV2.jar**

集成hasoffers
-------------

添加meta-data声明: ::

	<meta-dataandroid:value="请填写您获得的Hasoffers的Advertiser ID" 
	android:name="337_HASOFFERS_APPID"></meta-data>
	<meta-data android:value="请填写您获得的Hasoffers的Key" 
	android:name="337_HASOFFERS_APPKEY"></meta-data\

引入 **MobileAppTracker.jar**

添加receiver声明: ::

	<receiver android:name="com.mobileapptracker.Tracker" 
	android:exported="true">
		<intent-filter>
			<action android:name="com.android.vending.INSTALL_REFERRER" />
		</intent-filter>
	</receiver> 
	
集成FIKSU
---------

添加meta-data声明: ::

	<meta-data android:value="on" android:name="337_FIKSU_ON"></meta-data>
	
添加receiver声明: :: 

	<receiver android:name="com.fiksu.asotracking.InstallTracking" 
	android:exported="true">
		<intent-filter>
			<action android:name="com.android.vending.INSTALL_REFERRER" />
		</intent-filter>
	</receiver>
	
引入 **FiksuAndroidSDK_2.0.2.jar**

**注意若需要额外的receiver，需要在fiksu的receiver中加入如下格式的meta-data：** ::

	<meta-data android:name="forward.1" android:value="receiver类" />
	
比如同时接入hasoffers和FIKSU，需要将hasoffers的receiver以meta-data方式添加：::

	<receiver android:name="com.fiksu.asotracking.InstallTracking" 
	android:exported="true">
		<intent-filter>
			<action android:name="com.android.vending.INSTALL_REFERRER" />
		</intent-filter>
	<meta-data android:name="forward.1" android:value="com.mobileapptracker.Tracker" />
	<!-- 其他的receiver -->
	<meta-data android:name="forward.2" android:value="com.yourpackage.SomeReceiver" />
	</receiver>

自定义初始化
------------

若应用不想采用在AndroidManifest.xml文件中加meta-data的方式进行统一初始化，则可以调用SDK提供的自定义初始化方法， SDK提供了initXXXX的方法来方便应用调用，比如 ::

	SdkCore.initTapjoy(Context context,String appID,String secretKey);
	
传递相应的参数后即可进行初始化。 **应用可以自由决定如何使用该类方法**

其他方法
--------

* 部分第三方SDK提供一些事件统计，337SDK整合了部分方法

 注册事件统计：::

	 SdkCore.registerAction(context);
	 
 购买事件统计：::
 
	 SdkCore.Purchases(context, user, gross, currency);
	 
 user为购买人，gross为购买金额，currency为货币类型
 
* 获取Chartboost: ::

	SdkCore.getChartboost();
	
* 获取MobileAppTracker: ::

	SdkCore.getMobileAppTracker():
	
* 是否初始化完成: ::

	SdkCore.isInitFinish();
	