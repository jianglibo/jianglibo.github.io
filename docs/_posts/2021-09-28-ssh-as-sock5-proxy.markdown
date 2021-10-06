---
layout: post
title:  "如何使用SSH建立一个SOCK5代理"
date:   2021-07-30 20:59:04 +0300
categories: ssh
---
超级简单，只要在命令行输入：
{% highlight bash %}
ssh -v -D 1337 -N uc2ecca992df94ad490abf7e3b3a39455@xx.xx.xx.xx -p 2222
{% endhighlight %}
即可。其中1337是本地监听端口，uc2ecca992df94ad490abf7e3b3a39455@xx.xx.xx.xx是用户名和IP地址， 2222是远程端口。
当提示密码时输入密码。如果一切顺利就会保持在链接状态。

免费的账号和密码可以在[【此网站】][resp-me]上获取。

## CURL
{% highlight bash %}
curl --socks5 localhost:1337 ifconfig.io/all
{% endhighlight %}

输出：
{% highlight json %}
{
  "country_code":"FR",
  "encoding":"gzip",
  "forwarded":"163.172.144.249",
  "ifconfig_hostname":"ifconfig.io",
  "ip":"163.172.144.249",
  "lang":"",
  "method":"GET",
  "mime":"*/*",
  "port":56566,
  "referer":"",
  "ua":"curl/7.68.0"
}
{% endhighlight %}
注意country_code是法国。

## 设置firefox。
注意SOCKS主机必须填写你运行ssh命令的服务器，通常情况下是本机。即127.0.0.1，端口就是前面的-D参数后面的数值。
![setup firefox proxy](/assets/images/firefox-config.png)



[resp-me]: https://resp.me
