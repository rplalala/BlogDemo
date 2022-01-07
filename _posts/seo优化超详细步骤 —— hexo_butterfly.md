---
title: seo优化超详细步骤 —— hexo_butterfly 
abbrlink: 8da89045
date: 2022-01-06 17:40:35
tags:
  - 博客
  - seo
  - 踩坑
categories: 博客
---

优化过程中踩了不少坑，这里记录一下，也希望能帮助到需要的人。


# 优化文章url，便于收录

## 缩减链接长度 且 固定链接
hexo中，文章链接默认是 `permalink: :year/:month/:day/:title/` 这种形式。
即是：sitename/year/mounth/day/title四层的结构，且每次更改文章可能会导致链接变化

{% note info modern %}
搜索引擎认为对于一般的中小型站点，3层足够承受所有的内容了，所以蜘蛛经常抓取的内容是前三层，而超过三层的内容蜘蛛认为那些内容并不重要，所以不经常爬取。出于这个原因所以permalink后面跟着的最好不要超过2个斜杠。
{% endnote %}

而我们的链接由于大多带有中文，转码的时候就会很丑很长，并且更改文章可能还会导致链接变化，有没有一种办法，既可以缩短链接长度又固定链接呢？

利用 **abbrlink** 插件可以实现，它会在文章生成一个abbrlink字段，这个字段永远不会改变，且使链接层次短，利于seo收录

## 配置abbrlink
### 在hexo根目录安装：

    npm install hexo-abbrlink --save

### 更改hexo配置文件`_config.yml`

```yml
abbrlink:   # 新增abbrlink配置
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
permalink: posts/:abbrlink.html   #修改查找代码permalink
```
{% note info modern %}
百度收录规则里面明确强调了，需要html结尾的，必须html结尾，规范化url，有利于抓取并提高排名。
所以 :abbrlink.html 会比 :abbrlink 更好一点（？）。但其实不弄也没差
{% endnote %}

{% note info modern %}
abbrlink编码规则决定后就不要再改了，不然编码不统一，导致文章链接改变
{% endnote %}

### 效果如下
1. 生成的abbrlink字段
![](https://s2.loli.net/2022/01/07/k7b9JlqACX8GeMT.png)
2. 链接效果
![](https://s2.loli.net/2022/01/07/57CDzG6spPWFjlw.png)

{% note info modern %}
固定链接抛开seo优化不谈，本身就很重要，它保证了你链接的完好性
{% endnote %}

# 将网站提交给百度搜索引擎

{% note info modern %}
这里只谈百度了，毕竟百度才是国内主要用的，谷歌有墙
{% endnote %}

## 安装站点地图sitemap

{% note info modern %}
站点地图是一种文件，一般为xml形式，您可以通过该文件列出您网站上的网页，从而将您网站内容的组织架构告知Google和其他搜索引擎。Googlebot等搜索引擎网页抓取工具会读取此文件，以便更加智能地抓取您的网站。
{% endnote %}

### 先安装插件
```
npm install hexo-generator-sitemap --save         谷歌
npm install hexo-generator-baidu-sitemap --save   百度
```

貌似还需要在根目录的_config.yml中添加sitemap的路径？但是我不加也能在public中生成，酌情添加吧
```yml
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```

### 编译博客并生成xml文件
`hexo g `编译博客后，你会发现你的`public`中多了两个xml文件，分别是`sitemap.xml`以及`baidusitemap.xml`，里面收录着你博客将被爬虫的链接，此时代表你成功了。

之后再`hexo d`部署到网站上，`https://dingzihang00.top/baidusitemap.xml` 和 `https://dingzihang00.top/sitemap.xml` 验证一下能否跳转即可

## 添加并验证网站

**有三种方式：**

{% note info modern %}
添加时让你最好带www，其实不带也问题不大现在
{% endnote %}

1. 文件验证：得放在themes的source下？因为放到根目录的source下会破坏原有的格式导致验证失败。这里我也不是很懂QAQ
2. html标签验证：我是butterfly主题，自带了html标签站长验证，只需要填入content就行了

```yml
# Verification (站長驗證) 
# --------------------------------------
# 用于注册时验证该站点是不是你的（选HTML标签验证），不是指你的API token
site_verification:
  - name: baidu-site-verification
    content: xxx
  - name: google-site-verification
    content: xxx
```

3. CNAME验证：如果你有域名的话，那么你肯定也会域名解析了，这点我就不说了

**验证成功后我们就可以设置推送啦^^**

## 设置推送

**大致可以分为三种：**
1. 主动推送：最快，在你 `hexo g -d` 时会生成一个txt文件，里面记录着你最新提交的n个链接，并把其交给百度。

2. 自动推送：最方便，每当有人浏览你网页时，自动推送给百度。

3. sitemap：传统，将之前生成的sitemap文件交给百度，百度会定期检查你提交的sitemap文件并进行处理，但收录速度并不快。

{% note info modern %}
其实还有一个手动提交，让你一个个去手动提交。
但是，何必呢……
{% endnote %}

虽然从效率上来说，主动推送>自动推送>sitemap。但是其实三者可以一起使用，相辅相成，没必要分个高下。

### 主动推送

{% note info modern %}
据说可以解决百度无法爬虫github的问题，因为是我们自己提交给百度的
{% endnote %}

#### 安装插件 hexo-baidu-url-submit

在hexo根目录安装 `npm install hexo-baidu-url-submit --save`

#### 在根目录的_config.yml中添加配置
* 先添加 `hexo-baidu-url-submit` 插件的配置

```yml
baidu_url_submit:
  count: 3 ## 提交最新的多少个链接
  host: dingzihang00.top ## 在百度站长平台中注册的域名
  token: token ## 请注意这是您的秘钥， 所以请不要把博客源代码发布在公众仓库里!
  path: baidu_urls.txt ## 文本文档的地址， 新链接会保存在此文本文档里
```
**这里的token位推送接口给你的token**
![](https://s2.loli.net/2022/01/07/n5dGXUbTWx7q2g1.png)
 
{% note info modern %}
`count` 的值不要设太高，除非你真的有那么多的链接需要提交。否则多次提交重复内容百度会暂时将你加入黑名单或给你限制提交的次数甚至降低你网站的权值。而且在你调试网页且并没有新链接需要提交时，最好将count的值设为0，防止反复提交
{% endnote %}

* 还需要将_config.yml配置中的url改成你在站长平台注册的域名，不过我相信这个你肯定已经弄过了
```yml
url: https://dingzihang00.top
```

* 最后修改一下deployer，让你推送时顺便推给百度
```yml
deploy:
- type: git
  repository: https://xxxxxx.git
  branch: main
- type: baidu_url_submitter #把链接提交到百度站长平台
```

{% note info modern %}
注意 yml 文件的缩进是由严格要求的，为2个空格，不然hexo编译时会报错
{% endnote %}

#### hexo d -g 推送博客
都配置好后 `hexo g -d` 推送博文，出现这个就算成功啦！！
![](https://s2.loli.net/2022/01/07/6pwz9BkYiGgCAZX.png)

{% note info modern %}
remain：一天内剩余提交量
success：本次提交成功的链接数
{% endnote %}

### 自动推送
根据百度原话，只要将 js代码 放到博客的每一个页面下就行了
```js
(function(){
    var bp = document.createElement('script');
    var curProtocol = window.location.protocol.split(':')[0];
    if (curProtocol === 'https') {
        bp.src = 'https://zz.bdstatic.com/linksubmit/push.js';        
    }
    else {
        bp.src = 'http://push.zhanzhang.baidu.com/push.js';
    }
    var s = document.getElementsByTagName("script")[0];
    s.parentNode.insertBefore(bp, s);
})();
```
**问题是怎么把这段代码放到每一个页面下？**

网上大都只有 `next` 主题的方法，在主题配置文件中，令 `baidu_push 设置为 true` 即可，非常简单。而我是 `butterfly` 主题的该怎么做呢？

其实也非常简单，只需要利用主题配置文件中的 `inject` 配置项，在`head` 和 `body` 间引入这段 js代码 即可。在 themes/butterfly/source/js/ 下新建 `baidu_sub.js` ，将代码复制粘贴进去，然后在主题配置文件的 `inject` 中引入即可。

```yml
inject:
  head:
    # 百度自动提交js，将其放到每一个html页面的 <head> </head> 处 
    - <script defer src="/js/baidu_sub.js"></script>
  bottom:
    # 放到 bottom 处也行，这样就是放到每个页面的 </body> 前了
    #- <script defer src="/js/baidu_sub.js"></script>
```

**可以看到，这段js成功出现在了每个html页面的head中**
![](https://s2.loli.net/2022/01/07/whkgvaUEHSXRQ1y.png)

### sitemap推送

很简单，把你之前生成的 `sitemap.xml` 在百度上提交即可
![](https://s2.loli.net/2022/01/07/wNZas74vhGHkMIp.png)

到此，主动推送三种方式具体如何实现到介绍完了，呼~

## 拓展：robots.txt 文件的设置

**以下是百度原话：**
```txt
一、使用说明
  1. robots.txt可以告诉百度您网站的哪些页面可以被抓取，哪些页面不可以被抓取。
  2. 您可以通过Robots工具来创建、校验、更新您的robots.txt文件，或查看您网站robots.txt文件在百度生效的情况。
  3. Robots工具目前支持48k的文件内容检测，请保证您的robots.txt文件不要过大，目录最长不超过250个字符。

二、什么是robots文件
  Robots是站点与spider沟通的重要渠道，站点通过robots文件声明本网站中不想被搜索引擎收录的部分或者指定搜索引擎只收录特定的部分。

  搜索引擎使用spider程序自动访问互联网上的网页并获取网页信息。spider在访问一个网站时，会首先会检查该网站的根域下是否有一个叫做 robots.txt的纯文本文件，这个文件用于指定spider在您网站上的抓取范围。您可以在您的网站中创建一个robots.txt，在文件中声明 该网站中不想被搜索引擎收录的部分或者指定搜索引擎只收录特定的部分。

  请注意，仅当您的网站包含不希望被搜索引擎收录的内容时，才需要使用robots.txt文件。如果您希望搜索引擎收录网站上所有内容，请勿建立robots.txt文件。

三、robots.txt文件放在哪里?
  robots.txt文件应该放置在网站根目录下。举例来说，当spider访问一个网站（比如 http://www.abc.com）时，首先会检查该网站中是否存在http://www.abc.com/robots.txt这个文件，如果 Spider找到这个文件，它就会根据这个文件的内容，来确定它访问权限的范围。
```

总的来说，就是如果你不想让百度爬你某些链接，你就得设置 robots.txt 文件

{% note info modern %}
设置 robots.txt 可以防止大量垃圾链接提交给百度，提高你文章的权重
而且听说设置 rotbot.txt 可以提高收录概率，所以就算你没有不希望被爬取的内容，也可以设置一个空的 robots.txt 文件
{% endnote %}

具体用法用例，可以参照 https://ziyuan.baidu.com/college/courseinfo?id=267&page=12#h2_article_title28 。我这里只进行个例的演示

1. **若你没有不希望被爬取的内容**
```txt
User-agent: *
Allow: /

Sitemap: https://dingzihang00.top/baidusitemap.xml
Sitemap: https://dingzihang00.top/sitemap.xml
```

2. **若你有不希望被爬取的内容**
   
比如说，我的文章全部放在 `/posts` 中，那么我就可以只让百度爬取我 `/posts` 中的内容，具体设置如下。
```txt
User-agent: *
Allow: /posts
Disallow: /

Sitemap: https://dingzihang00.top/baidusitemap.xml
Sitemap: https://dingzihang00.top/sitemap.xml

```

{% note info modern %}
1. Allow 的优先级大于 Disallow
2. 最后可以放你的 sitemap.xml 文件链接
{% endnote %}


你可以通过以下步骤验证你的 `robots.txt` 文件是否配置成功

1. **`hexo g -d` 后输入链接，看能否成功跳转到你的 `robots.txt`**
![](https://s2.loli.net/2022/01/07/6jJkYTofUFsrcBq.png)

2. **在百度站长平台 robots 验证中检查**

![](https://s2.loli.net/2022/01/07/w5c7MUpnov4sb98.png)
![](https://s2.loli.net/2022/01/07/Xd5vA6FbWrSUx7H.png)


总算写完了，这算是我写最长的一篇文章了，好累ヽ(￣▽￣)و。

{% note info modern %}
就算配置好后，如果你是 github pages 上托管的网站，可能不被收录。
可以采用 cdn 加速域名 或 部署一个国内镜像站 等方法解决。
这里就不介绍了，自行百度
{% endnote %}




