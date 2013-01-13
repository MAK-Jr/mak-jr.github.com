---
layout: post
title: "Ruby gems歷險記"
date: 2013-01-13 16:10
comments: true
categories: 
---
想安裝Cloudfoundry的command line tool,所以就這樣來做

`sudo gem install vmc`

失敗,提示

{%codeblock%}
ERROR:  While executing gem ... (Zlib::GzipFile::Error)
    not in gzip format
{%endcodeblock%}

懷疑是網絡問題,所以查看gem的sources,運行

`gem sources`

輸出:

{% codeblock %}
*** CURRENT SOURCES ***

http://rubygems.org/
http://ruby.taobao.org
http://gems.rubyforge.org/
{% endcodeblock %}

依次把其他sources都刪除,只留 `http://rubygems.org/`

{% codeblock %}
gem sources -r http://ruby.taobao.org
gem sources -r http://gems.rubyforge.org/
{% endcodeblock %}

再次運行:

`sudo gem install vmc`

成功!


