---
layout: post
title: "農行網上支付IE證書問題的解決"
date: 2013-06-07 17:07
comments: true
categories: 
---
今天想用 paypal 來支付 digitalocean 的 VPS,卡在農行的 IE 證書登陸頁面.客户的浏览器有拦截提示，拦截的文件为Netsign.Cab，提示为未能识别发行者.

解決的方法如下,特此記錄.

* 打开IE浏览器，在菜单栏里选择“工具”然后选择“INTERNET选项”
* 在打开的“INTERNET选项”对话框中选择“安全”选项卡，然后选择“可信站点”再点击“站点”
* 分别将下面3个站点添加进信任站点

	`https://easyabc.95599.cn`
	
	`https://*.95599.cn`
	
	`https://www.95599.cn`
	
* 点击“可信站点”然后再点击“自定义级别(C)...”对Activex控件进行设置。這裡為了簡單,可以把所有關於ActiveX的設置都修改為啟用(enable),為了瀏覽器的安全上網,支付後,請修改為默認設置.

重啟瀏覽器後一切正常使用.