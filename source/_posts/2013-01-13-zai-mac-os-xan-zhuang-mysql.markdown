---
layout: post
title: "在Mac OS X安裝Mysql"
date: 2013-01-13 18:26
comments: true
categories: 
---
簡單安裝

1. 使用HomeBrew安裝mysql

	`brew install mysql`
	
2. 配置數據庫
	
	{% codeblock %}
	$ mysql_install_db --verbose --user=`whoami` --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmpdir=/tmp

	{% endcodeblock %}
	
-=Done=-