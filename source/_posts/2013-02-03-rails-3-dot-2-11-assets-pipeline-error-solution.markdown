---
layout: post
title: "Rails 3.2.11 assets pipeline error solution"
date: 2013-02-03 21:24
comments: true
categories: 
---
默認情況下,運行`rake assets:precompile`, 打開頁面後, rails會提示找不到application.js等等一些assets, 出現這種情況的原因是在production模式下運行的public目錄是沒有expose給用戶的,這樣設計是由ngix和apache這些web server來接管public目錄, 所以,如果只是用rails默認的server來跑的話,需要到`config/environments/production.rb`修改一行, 讓public目錄expose出來.

`config.serve_static_assets = true`

-=[完]=-