---
layout: post
title: "在Rails中使用Pry來做調試"
date: 2013-01-07 15:07
comments: true
categories: 
---
* 首先安裝Pry這個Gem

	`sudo gem install pry`
* 其他參考這篇文章

　[Using Pry With Rails](http://filipemoreira.com/blog/2012/03/09/using-pry-with-rails
)

* 使用方法

{% codeblock lang:ruby %}
	binding.pry
{% endcodeblock %}

　　可以在Model, Controller, View裡面使用


	 