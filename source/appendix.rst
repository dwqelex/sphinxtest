游戏内客服系统实现的最佳方式 Step by Step
---------------------

- 概述

	因为咱们使用的客服系统Freshdesk与用户的往来最好的方式是基于邮件，故而在此基础上设计了这份客服最佳实践


- 为游戏注册一个客户服务邮箱

	前期大家可以自行注册，@337.com, @elex-tech.com 后缀的邮箱找IT开通即可。 计划中可能会开启一个@support.337.com的邮箱后缀。游戏也可自行注册@Gmail.com后缀的邮箱来用于接收用户发来的客服邮件


- 在客服系统中设置邮箱及转发

	客服人员可以在Freshdesk中创建一个产品，并未此产品指定若干个来信邮箱。在添加上一步注册好的客服邮箱之后，Freshdesk会提供一个自己的邮件地址用于接收转发过来的邮件，这时，去我们第一步注册的邮箱里，设置收到的邮件转发到提供的这个地址即可。

	部分邮件服务商在做转发的时候，会向目标邮箱发一个验证，这个验证可以直接在Freshdesk中收到并点击其中的连接进行认证即可。


- 在游戏中提供按钮让用户方便的提交问题

	游戏中可以提供一个联系客服按钮，点击这个按钮时，我们可以预先吧收件人（也就是我们之前注册的邮箱）等信息填好，游戏也可以把对解决用户问题有帮助的内容（用户UID，系统版本号，游戏版本号等）预先写在邮件正文中，并提示用户这些信息不要删除。

	用户可以自行填写邮件标题，并在正文中附上自己问题的描述后发送邮件。 这封邮件就会以tickets形式出现在Freshdesk后台。

	后续客服的回复和用户的问题补充就都通过邮箱系统了。

- 按钮实现方式
	Android: ::

		Intent intent = new Intent(Intent.ACTION_SENDTO,Uri.parse(MailTo.MAILTO_SCHEME));
		intent.putExtra(Intent.EXTRA_SUBJECT, subject);//主题
		intent.putExtra(Intent.EXTRA_TEXT, text);//正文
		intent.putExtra(Intent.EXTRA_EMAIL, to);//收件人
		startActivity(Intent.createChooser(intent, "发送邮件"));
	

	系统会自动提示玩家选择相应的邮件客户端去发送。剩下的事就可以不用管了，发送完邮件后会自动返回游戏。

	iOS: ::

		Coming Soon.
