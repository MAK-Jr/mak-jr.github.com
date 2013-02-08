---
layout: post
title: "Use Engine Yard service as a socket5 proxy"
date: 2013-02-08 10:22
comments: true
categories: 
---

Engine Yard 使用 Amazon Web Services(AWS) 作為服務器, 同時也提供了 SSH acess. 在大陸, 為了 bypass 國家的 firewall 去訪問一下被牆的網站, 通常都需要翻牆, 有了 AWS, 最最簡單的 bypass 方法, 就是用 ssh 通道作為 socket5 代理, 就行了.

所以, 建立好 instance 後, 直接用 `ssh -C2qTnN -D 8080 yourserver` 就可以在本地開一個 socket5 代理了. 配合 chrome 的代理外掛, 就可以順利翻牆了, 非常簡單.

`PS. Engine Yard 提供了500個小時的免費運行時間`

[Engine Yard](http://www.engineyard.com)