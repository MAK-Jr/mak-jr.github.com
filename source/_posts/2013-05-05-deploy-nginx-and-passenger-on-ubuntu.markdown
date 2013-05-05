---
layout: post
title: "Deploy Nginx and Passenger on Ubuntu"
date: 2013-05-05 09:34
comments: true
categories: 
---
1. 安装编译器和一些依赖模块(如果更新包比较慢,请更换为中国的源,具体看这里http://yubosun.akcms.com/tech/ubuntu-apt-get-sources-lst.htm)

	`sudo apt-get update`
	
	`sudo apt-get install -y build-essential openssl curl libcurl3-dev libreadline6 libreadline6-dev git zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf automake libtool imagemagick libmagickwand-dev libpcre3-dev libsqlite3-dev libmysql-ruby libmysqlclient-dev`
	
2. 安装 RVM
	
	`curl -L https://get.rvm.io | bash -s stable`
	
	`source ~/.rvm/scripts/rvm`
	
	修改到淘宝的服务器
	
	`cp $rvm_path/config/db $rvm_path/config/db.bak`
	
	`sed 's!ftp.ruby-lang.org/pub/ruby!ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db`
	
	安装缺失模块
	
	`sudo apt-get install git-core`
	
	`sudo apt-get install sqlite3 libgdbm-dev libncurses5-dev bison libffi-dev`
	
	安装 ruby 1.9.3
	
	`rvm install 1.9.3`
	
3. 设置 ruby 版本和 gemset

	`rvm use 1.9.3`
	
	`rvm gemset create rails3213`
	
	`rvm use 1.9.3@rails3213 --default`
	
4. 安装 passneger 和 nginx

	`gem install passenger`
	
	`rvmsudo passenger-install-nginx-module`
	
	`在接下来的选项依次选按 enter/1/enter`
	
5. 安装  rails

	替换淘宝的源
	
	`gem source -r http://rubygems.org/`
	
	`gem source -a http://ruby.taobao.org`
	
	`gem install rails`
	
6. 创建一个测试 nginx 的 rails 项目

	`mkdir ~/projects && cd ~/projects`
	
	`rails new demo`
	
	把 demo 目录下面的 Gemfile 第一行改为 source 'http://ruby.taobao.org'
	
	`cd demo`
	
	`bundle install`
	
	安装 node.js 作为 Rails 的 Javascript engine
	
	`sudo apt-get install nodejs`
	
	测试 rails 项目
	
	`rails s`
	
7.  配置 nginx 指向测试的 rails 项目

	编辑 /opt/nginx/conf/nginx.conf
	
	`sudo vi /opt/nginx/conf/nginx.conf`
	
	注释掉 server 里面的东西,写成这样:
	
	{% codeblock lang: ruby %}
	
	server{
        listen       80;
        server_name  localhost;

        root /home/ubuntu/projects/demo/public;
        passenger_enabled on;
        rails_env production;
    }
    
	{% endcodeblock %}
	
上面的 root 后面指向的是测试的 rails 项目里面的 public 目录,在这个配置文件的最上面加入:
	
    `user root;`
        
8. 配置测试的 rails 项目

	`cd ~/project/demo`
	
	`echo “rvm use 1.9.3@rails3213”  > .rvmrc (这个可选)`
	
	`rvm rvmrc trust`
	
	增加 config/setup_load_path.rb, 贴上以下代码并且保存:
	
	{% codeblock lang: ruby %}
if ENV['MY_RUBY_HOME'] && ENV['MY_RUBY_HOME'].include?('rvm')
  begin
    gems_path = ENV['MY_RUBY_HOME'].split(/@/)[0].sub(/rubies/,'gems')
    ENV['GEM_PATH'] = "#{gems_path}:#{gems_path}@global"
    require 'rvm'
    RVM.use_from_path! File.dirname(File.dirname(__FILE__))
  rescue LoadError
    raise "RVM gem is currently unavailable."
  end
end

# If you're not using Bundler at all, remove lines bellow
ENV['BUNDLE_GEMFILE'] = File.expand_path('../Gemfile', File.dirname(__FILE__))
require 'bundler/setup'
	{% endcodeblock %}
	
然后启动nginx:
	
	`sudo /opt/nginx/sbin/nginx`
	
停止 nginx 可以用:
	
	`sudo /opt/nginx/sbin/nginx  -s stop`
	
- 全文完
	
	