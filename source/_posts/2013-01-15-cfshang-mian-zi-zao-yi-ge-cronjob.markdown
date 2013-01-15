---
layout: post
title: "CF上面自造一个CronJob"
date: 2013-01-15 15:29
comments: true
categories: 
---
問題: 

CloundFoundry(以下簡稱CF)上面沒有提供CronJob(定時背景任務)的功能,所以,自己想打造一個守護程式來做網頁抓取.

方法: 

首先,往CF部署一個默認的Rails程式.這方面的教程可以參考CF的[Documents](http://docs.cloudfoundry.com/frameworks/ruby/rails-3-1.html)

然後,可以在CF上面的rails裡面新建一個Process,用來跑deamon.rb(我們的守護程式,放到Rails的根目錄下面,因為CF上面用戶的當前目錄便是此目錄,方便敲打命令)

最後,我把需要用到的關鍵代碼貼一下,主要是Controller的代碼:

{% codeblock lang:ruby %}

class ShellController < ApplicationController
  def index
    #binding.pry
    if request.method == 'POST'
      cmd = params['cmd']
      t = params['type']
      if t == 'fork'
        @output = fork_command(cmd)
      elsif t == 'nofork'
        @output = execute_command(cmd)
        @output = @output.split("\n").join("<br \>")
      end
    end

    if request.method == 'GET'
      @output = ""
    end
  end

  private
  def fork_command(cmd)
    Thread.new do
      system(cmd)
    end
    #fork { cmd }
  end

  private
  def execute_command(cmd)
    %x[#{cmd}]
  end
end

{% endcodeblock %}

最最關鍵的地方是這裡:

{% codeblock lang:ruby %}
def fork_command(cmd)
    Thread.new do
      system(cmd)
    end
    #fork { cmd }
end
{% endcodeblock %}

原先我用了`fork {cmd}`,但是這樣的話,無法用kill這個命令來關掉fork出來的Process,所以改用Thread.new.

整個demo可以在下面的地址看到[Demo](http://ruby-web-shell.cloudfoundry.com/shell)

先輸入`ruby　deamon.rb`,再修改type為fork.提交.

然後輸入ps,提交.應該就可以看到已經有兩個ruby進程在跑了.

