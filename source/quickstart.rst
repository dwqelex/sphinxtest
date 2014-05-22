
引入依赖工程
--------------

	337SDK Android版本以library工程的形式提供，使用时必须引入核心Lib工程。

	.. image:: _static/includelib.jpg

	用户模块涉及到Facebook登录部分，所以还需要引入Facebook SDK。下载地址：https://developers.facebook.com/docs/android/

添加权限  
-------- 
	
需要声明的权限如下: ::
	
	<!-- 基本权限 -->
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.GET_ACCOUNTS" />

	<!-- GoogelPlay内购功能 -->
	<uses-permission android:name="com.android.vending.BILLING" />

	<!-- 韩国Tstore平台 -->
	<uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"/>
    <uses-permission android:name="com.tmoney.vending.INBILLING"/> 
    <permission android:name="com.tmoney.vending.INBILLING"/>
	
	<!-- Fortumo手机短信付款功能 -->
	<uses-permission android:name="android.permission.RECEIVE_SMS" />
	<uses-permission android:name="android.permission.SEND_SMS" />

	<!-- KT平台 -->
    <uses-permission android:name="android.permission.GET_TASKS"/> 

 
添加Activity、MetaData和其他内容 
--------------------------------
需要添加的内容如下: ::

        <activity
            android:name="com.web337.android.widget.PopuView"
            android:theme="@style/Theme.Translucent"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:windowSoftInputMode="adjustResize">
        <activity
            android:name="com.web337.android.pay.PayCoreMobileActivity"
            android:configChanges="orientation|keyboardHidden|screenSize" 
            android:theme="@style/mobile337transparent">
        </activity>
        <activity
            android:name="com.web337.android.pay.PayShowPackagesActivity"
            android:configChanges="orientation|keyboardHidden|screenSize" 
            android:theme="@style/mobile337transparent">
        </activity>
        <activity
            android:name="com.web337.android.ticket.TicketCoreActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:windowSoftInputMode="adjustResize" >
        </activity>
        <activity
            android:name="com.fortumo.android.FortumoActivity"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" >
        </activity>
        <activity
            android:name="com.web337.android.pay.fortumo.FortumoActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:theme="@android:style/Theme.Translucent.NoTitleBar" >
        </activity>
        <activity
            android:name="com.web337.android.widget.Web"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:launchMode="singleTask" >
        </activity>
        <activity
            android:name="com.web337.android.user.UserPage"
            android:theme="@style/mobile337user" >
        </activity>
        <activity
            android:name="com.web337.android.user.GoogleAcountLogin"
            android:configChanges="orientation|keyboardHidden|screenSize" >
        </activity>
        <activity
            android:name="com.facebook.LoginActivity"
            android:theme="@android:style/Theme.Translucent" />
        <activity 
            android:name="com.skplanet.dodo.IapWeb"
            android:configChanges="orientation|keyboardHidden|locale|screenSize|layoutDirection"
            android:excludeFromRecents="true" 
            android:windowSoftInputMode="stateHidden"/>

        <!-- KT平台 -->
        <service android:name="com.kt.olleh.inapp.TimerService" />
        <activity android:name="com.web337.android.pay.kt.KTActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"></activity>
        
        <!-- Naver -->
        <activity android:name="com.web337.android.pay.naver.NaverActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:theme="@android:style/Theme.Translucent.NoTitleBar"></activity>
            
        <receiver
            android:name="com.web337.android.Tracker"
            android:exported="true" >
            <intent-filter>
                <action android:name="com.android.vending.INSTALL_REFERRER" />
            </intent-filter>
        </receiver>
        <receiver android:name="com.fortumo.android.BillingSMSReceiver" >
            <intent-filter>
                <action android:name="android.provider.Telephony.SMS_RECEIVED" >
                </action>
            </intent-filter>
        </receiver>

        <service android:name="com.fortumo.android.FortumoService" />
        <service android:name="com.fortumo.android.StatusUpdateService" />
        <uses-feature android:name="android.hardware.telephony" android:required="false"></uses-feature>
        <meta-data
            android:name="com.facebook.sdk.ApplicationId"
            android:value="\ 220782057940018" />
        <meta-data android:name="iap:api_version" android:value="1"/> 
		
*screenSize添加时如果出现错误，请更改targetSdkVersion为13以上即可*

*com.facebook.sdk.ApplicationId如果不使用默认的220782057940018，可以替换为游戏自己的ID，需要事先将应用secret配置在337的后台*	

*如果需要添加的INSTALL_REFERRER的receiver不止一个，可以单独设立一个统一的入口，然后转发给com.web337.android.Tracker*

添加第三方广告推广平台SDK
---------------

具体配置见第三方广告SDK条目


SDK初始化以及重载关键方法
----------------
在主Activity的onCreate中调用::

		FuncCore.onCreate(this);
		
在主Activity的代码中，重载以下方法::

		@Override
		protected void onDestroy() {
			FuncCore.onDestroy(this);
			/*your code*/
			super.onDestroy();
		}

		@Override
		protected void onStart() {
			FuncCore.onStart(this);
			/*your code*/
			super.onStart();
		}

		@Override
		protected void onStop() {
			FuncCore.onStop(this);
			/*your code*/
			super.onStop();
		}

		@Override
		public void onBackPressed() {
			if (FuncCore.onBackPressed(this)) {
				return;
			} else {
				/*your code*/
				super.onBackPressed();
			}
		}

设置支付回调
------------
 ::

	FuncCore.setPayCallback(new FuncCore.PayCallback() {
				
		@Override
		public void onInitFinish(Msg msg) {
			if(msg.isSuccess()){
				/*初始化成功*/
			}else{
				/*初始化失败*/
			}
		}
			
		@Override
		public void onComplete(Order o) {
			/*付款成功*/
		}
		@Override
		public void onCancel() {
			/*取消支付*/
		}
		@Override
		public void onFailed(Msg msg) {
			/*支付失败*/
		}
	});
		
用户登录
------------
*  实例化一个回调对象： ::
	
		final FuncCore.LoginCallback callback = new FuncCore.LoginCallback(){
			@Override
			public void onLoginSuccess(User u, boolean isRegist) {
				if(isRegist){
					/*注册成功*/
				}else{
					/*登录成功*/
				}
			}
					
			@Override
			public void onCancel() {
				/*取消登录*/
			}
		};
		
*  调用登录方法： ::
		
		FuncCore.goLoginAndInit(Context c, FuncCore.LoginCallback callback,final boolean priorityLogin);

*可以在进入游戏主页面后直接调用，Context传递当前的activity即可，LoginCallback传递上一步创建的callback对象*

**调用该方法后只需要关心callback中的两个回调方法即可，若当前无登录用户，则会弹出登录或注册页面，用户登录或注册完成后，会回调。若已经有登录的用户，则直接回调**

设置角色和服信息
------------

	在获取到角色的信息和所在服之后，设置一下相应的信息： ::

		com.web337.android.id.Zone.getInstance().clear();
		com.web337.android.id.Zone.getInstance().setRole_id("roleid00001");
		com.web337.android.id.Zone.getInstance().setRole_name("wangxiaoming");
		com.web337.android.id.Zone.getInstance().setServer_id("1");
		com.web337.android.id.Zone.getInstance().setServer_name("ServerName");

加入行云统计
------------

用户登录完成后，设置完角色信息即可调用: ::

	com.web337.android.sdks.XA.send(Context c);
	
Context传递当前Activity即可

打开浮动窗口
--------------

进入游戏主面板后，打开337的浮动窗口: ::

		FuncCore.showFloatWindow(activity);


直接发起支付
--------
 ::

		Order o = new Order();
		o.setAmount(游戏币数量);
		o.setDescription(商品描述);
		o.setGross(商品金额);
		o.setCurrency(货币类型);
		o.setProductId(商品代码);
		PayCore.beginPay(activity, o);


展示支付套餐
--------

需要如下三个步骤: ::

		//展示套餐，此处套餐均在支付平台后台配置
		PayCore.show();

*show()方法支持传递一个自定义字符串，该值最终会作为custom_data参数回调给游戏服务器*

**至此337 SDK接入完成，更多的使用方法可以参考各个模块的文档**


 