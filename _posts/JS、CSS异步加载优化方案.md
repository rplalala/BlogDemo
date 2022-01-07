---
title: JS、CSS异步加载优化方案
tags:
  - 优化
  - 博客
  - 转载
categories: 博客
abbrlink: b3f89ff3
date: 2022-01-05 16:40:21
---

{% note info modern %}
此文转载于作者 Akilar 的 Hexo异步加载方案，基本都是该文原话，只摘要我理解的部分
这里仅分享记录我的总结（免得忘了……）
详情请去 https://akilar.top/posts/615d5ede/
{% endnote %}

# JS异步加载

**例如：**

```yml
# 看板娘，外部js
<script defer src="https://cdn.jsdelivr.net/gh/rplalala/BlogDemo/live2d-widget/autoload.js"></script>

<script async src="//at.alicdn.com/t/font_3113985_1d98llbey06.js"></script>
```

## 关键字解释

### defer

1. 具有`defer`特性的脚本不会阻塞页面。
2. 具有`defer`特性的脚本执行总是要等到`DOM`解析完毕，但在`DOMContentLoaded`事件之前。
3. defer特性仅适用于外部脚本。如果 script 脚本没有src，则会忽略defer特性。

### async

**async特性意味着脚本是完全独立的：**

1. 浏览器不会因`async`脚本而阻塞（与`defer`类似）。

2. 其他脚本不会等待 `async` 脚本加载完成，同样,`async` 脚本也不会等待其他脚本。

3. DOMContentLoaded和异步脚本不会彼此等待：
   - `DOMContentLoaded` 可能会发生在异步脚本之前（如果异步脚本在页面完成后才加载完成）
   - `DOMContentLoaded` 也可能发生在异步脚本之后（如果异步脚本很短，或者是从`HTTP`缓存中加载的）。换句话说，`async` 脚本会在后台加载，并在加载就绪时运行。`DOM`和其他脚本不会等待它们，它们也不会等待其它的东西。
4. `async` 脚本就是一个会在加载完成时执行的完全独立的脚本。就这么简单，现在明白了吧？

## 使用分析

1. 不加任何`async`和`defer`的情况，页面总加载时间最长，是

   <u>HTML加载时间+下载脚本时间+执行脚本时间</u>

2. 加了`async`和`defer`的时间，**在加载HTML时间足够长的情况下**，所有静态资源总的加载时间都是

   <u>HTML加载时间+执行脚本时间</u>

## 应用场景
1. 尽可能给每个 script 标签都加上 defer 和 async。
2. 确定为独立脚本或原生脚本的情况下，使用 async。这一条并不适用于大型 js (例如 busuanzi.js)。原因可以看上面的原理拆解图，大型 js 的执行时间依然会造成大面积的 HTML 阻塞。
3. 如果实在不清楚应该添加哪个，则以 defer 求稳，确保依赖项不会缺失。
4. 总的来说，async 加在那些非必要的，起装饰或者优化效果的 js 上，defer 加在那些确保页面完整性的必要 js 上。
5. 来自 Heo 的建议，不要给影响页面生成的 js（例如 util.js、main.js、lazy_load.js、vue.js、jquery.js）添加异步加载标签 (不论是 async 还是 defer 都不要加)，不然会造成大面积页面功能模块失效。推测是由于部分 HTML 元素需要由 js 动态写入，抑或部分整合在各个 pug 中的 script 片段无法添加 defer 导致。


{% note info modern %}

我看完的理解是：
1. 除了大型JS 和 影响页面生成的JS，尽量都加上 `defer`。（大型JS例如：busuanzi.js，影响页面生成的JS例如：lazy_load.js、vue.js、jquery.js）
2. 需要依赖的JS，不要加 `async`。（例如：看板娘需要jquery依赖，不加 async、侧栏聊天窗用到了vue依赖，不加 async）
   
{% endnote %}

{% note info modern %}

当一个 js 文件中 引用了其他 js 文件，那么就成这个 js 文件存在依赖
   
{% endnote %}

再分享一篇关于 `async` 和 `defer` 的文章 https://segmentfault.com/a/1190000006778717

# ~~CSS异步加载~~ （可用CSS整合代替）

**例如：**

```yml
<link rel="stylesheet" href="/example.css" media="defer" onload="this.media='all'">
```

这里的 `media="defer" onload="this.media='all'" 为CSS实现异步加载` 为CSS实现异步加载


{% note warning no-icon %}
此处的**media="defer"**中的**defer**并无意义，是个**无效media**（只是方便理解才这么写）,目的是让浏览器认为这个CSS文件优先级非常低，从而在不阻塞的情况下进行加载。
{% endnote %}

# CSS整合
详情去 Akilar 店长那看就行了，再贴一次链接吧 https://akilar.top/posts/615d5ede/

这里讲一下我遇到的问题：

1.  在/themes/butterfly/source/css/ 路径下新建 `_custom` 后，`hexo g` 无法在 public 生成 `_custom` 文件夹。

![](https://s2.loli.net/2022/01/07/1THKDcdSE2Z59rU.png)

解决：把 `_custom` 改成 `custom` 就行了

![](https://s2.loli.net/2022/01/07/4APdCe5O2XLoSqY.png)

2. 有些样式和 `index.css` 里重复了，可能会被覆盖。（此处样式不包括 糖果屋 的样式）

解决：把你的样式包丢最下面，这样就算重复了，也是你的覆盖原有的


{% note warning no-icon %}
注意：若你的CSS还 @import 了字体包、图标包等，那么这个CSS应该放在最上面，不然你前面的样式将无法获得字体和图标
{% endnote %}


![](https://s2.loli.net/2022/01/07/j7dmGcYefWKip4D.png)



