---
layout: post
title: "Using git with https proxy"
date: 2013-07-02 13:09
comments: true
categories:
---

* 為什麼為git使用https proxy

大陸經常性的遇到github，bitbucket不能正常使用的情況，所以需要使用proxy。

* 如何使用

{% codeblock %}
git config --add http.proxy 127.0.0.1:8087
git config --add https.proxy 127.0.0.1:8087
git config --add https.sslVerify false
git config --global http.postBuffer 524288000
{% endcodeblock %}

8087是本地的goagent的端口號。

前提是必須使用git init後才能用上面的命令,因為會修改.git/config文件。

最後，修改本地的repo的origin或者其他的remote為https協議。


