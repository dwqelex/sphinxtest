

***用户反馈功能依赖于用户系统，即只有接入了用户登录模块且用户登录之后才能使用用户反馈功能***

在应用中引入lib工程
-------------------
请参照快速集成中（引入337lib工程）

在Manifest文件添加必要的声明
----------------------------
	
需要添加内容入下: ::

	<uses-permission android:name="android.permission.INTERNET"/>

	<activity
		android:name="com.web337.android.ticket.TicketCoreActivity"
		android:configChanges="orientation|keyboardHidden|screenSize"
		android:windowSoftInputMode="adjustResize" >
	</activity>
	
使用用户反馈功能
----------------
* 如果IdZone中已经设置了用户相关的游戏信息可以跳过这一步。调用``SupportCore.setUserInfo(String role_id, String role_name,String server_id, String server_name);``设置用户游戏角色服务器等相关信息。

* 调用``SupportCore.openTicket(Context context, boolean addTicket);``即可直接使用用户反馈功能。如果返回true则正常使用反馈功能。如果返回false说明用户游戏角色的相关信息没有设置。



