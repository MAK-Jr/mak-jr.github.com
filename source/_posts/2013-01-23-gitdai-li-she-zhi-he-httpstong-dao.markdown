---
layout: post
title: "git代理设置和https通道"
date: 2013-01-23 15:34
comments: true
categories: 
---
這幾天GFW的魔爪已經伸向github了,導致現在git已經無法使用,但是,如果你是使用goagent來翻牆的話,還是能解決這個問題的.

* 升級git(起碼要git version 1.7.10.4以上)

* 其次修改你本地git的remote

  {% codeblock %}
  git remote add origin https://github.com/user/repo.git
  {% endcodeblock %}
   
   這個根據你的repo而定,注意一定要用https,而不是ssh
   
* 修改git的http代理.
  
  {% codeblock %}
  git config --global http.proxy "127.0.0.1:8087"
  {% endcodeblock %}
  
  `127.0.0.1:8087`這個是goagent默認的代理服務器
  
* 最後,如果你`git push origin master`的話,會提示github的用戶名和密碼.

* 如果不打算用代理了,可以直接
　 {% codeblock %}
　 git config --global http.proxy ""
　 {% endcodeblock %}

	