---
title: Hexo+GitHub 实现域名配置（别被旧文章坑了）
tags:
  - 博客
  - 域名
categories: 博客
abbrlink: f77f8e26
date: 2022-01-03 14:26:49
---

# 买域名并实名认证
我是在阿里云买的，只要9元一年

# 域名解析

{% note default no-icon %}
网上大多都是CNAME 和 A 这两个记录类型搭配使用，这样是很容易出错的
{% endnote %}

## 进入阿里云域名解析管理
这样子设置即可
![域名解析.png](https://s2.loli.net/2022/01/03/ltIw6kUXRdpLuqr.png)

# 分析

## 解读
1. 将域名 www.dingzihang00.top 指向 rplalala.github.io
2. 将域名 dingzihang00.top 指向 rplalala.github.io

这样无论我输入 www.dingzihang00.top 还是 dingzihang00.top，都可以成功跳转到 rplalala.github.io 下了

## 为什么别用 A记录类型？

1. A记录类型是将域名指向一个IP
2. CANME记录类型是将域名指向另一个域名
   
**GitHub的IP是多变的，可能你这段时间是这个IP，过段时间就是另外一个IP了**

```text
18年改版后，GitHub有四个IP：
1. 185.199.108.153
2. 185.199.109.153
3. 185.199.110.153
4. 185.199.111.153
```

而网上教学多为Ping一个IP，然后通过A将申请的域名指向这个IP。例如：dingzihang00.top -> 185.199.108.153。但当GitHub IP发生变化时，例如变成 185.199.109.153 时，就对应不上从而导致跳转失败了。

CANME 就不会出现这个问题，因为无论IP怎么改，rplalala.github.io 都是与它IP一一对应的，

```text
例如：
    dingzihang00.top -> rplalala.github.io -> 185.199.108.153   更改IP前
    dingzihang00.top -> rplalala.github.io -> 185.199.109.153   更改IP后
```
