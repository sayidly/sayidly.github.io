---
title: "IFTTT, Instapaper 和 RSS 来收集和整理资讯"
published: true
---

为什么需要收集和整理自己的资讯呢？

> 1984 中有一句话是：__谁控制了过去就控制了未来；谁控制了现在就控制了过去。__<br/>
> 而现在的时代更像是：__谁控制了媒体，谁就控制了舆论，谁就控制了思想。__

当你的资讯来源比较单一的时候，就可能容易被错误的信息误导。丰富自己的资讯源会有利于自己从众多杂乱，甚至从掩人耳目的讯息中找到有价值的那些。

而整理资讯也有利于高效的收集到有用的讯息，不被过多的碎片化的资讯所困扰。

## 概念介绍（可以跳过）

**RSS 是什么？**

[RSS][RSS]（简易信息聚合）是一种消息来源格式规范，用以聚合经常发布更新数据的网站。英文全名是 Really Simple Syndication“简易信息聚合”。想要了解更多的同学可以看[这里][RSS]。

简而言之 RSS 就是一种定制个性化推送信息的服务，把你需要的一些信息送到你的口袋。而且基本上现在所有的资讯类网站都支持了 RSS。

总结一下，RSS 的好处有
* 收集资讯主动推送给你
* 不受“墙”的限制

**什么是 IFTTT？**

[IFTTT][IFTTT]，是一个新生的网络服务平台，通过其他不同平台的条件来决定是否执行下一条命令。即对网络服务通过其他网络服务作出反应。IFTTT得名为其口号“if this then that”。

IFTTT 可以玩的东西有很多，但是这里只谈论通过 IFTTT 来触发 RSS 并推送至 Instapaper。

**什么是 Instapaper?**

[Instapaper][Instapaper] 一款 "之后再读" 的应用。最近也是刚好走过了十年，一直是个人非常喜欢的一个小而美的应用。虽然在 [17 年被 Pinterest 收购了][instapaper_pinterest]，但幸运的是感觉产品还是没有被他们带错方向，近期的一些优化都是往用户体验好的方向走。

更新：[18 年时候又从 Pinterest 中，独立出来了][Instapaper_independent]。我也因此买了会员来支持他们。


## 步骤

**1. 寻找网站的 RSS 链接**

大部分的资讯类网站都有提供了 RSS 的链接。

以《[纽约时报中文版][cn_nytimes]》网站为例。滑动至网站底部，角落有一个 「RSS」。点击之后就打开了一个新的页面，网址是 https://cn.nytimes.com/rss/ ，如下

![cn nytimes rss]({{ site.baseurl }}/assets/images/cn-nytimes-rss.jpg){: .medium}

**2. 注册 Instapaper**

- [Instapaper][Instapaper] 

Instapaper 用来存放收集到的资讯。

**3. 注册 IFTTT**

- [IFTTT][IFTTT]

用 IFTTT 来做对应的触发操作。即**当有新的 RSS 的进来时候，就触发推送至 Instapaper**，也就是 IFTTT 的机制 **If This Then That**

![cn nytimes rss]({{ site.baseurl }}/assets/images/if-this-then-that.jpg){: .medium}

**4. 连在一起**

完成以上步骤之后，就回到 [IFTTT][IFTTT] 中来创建触发器。

1. 在 IFTTT 中，点击 Explore -> Make your own Applets from scratch。 
1. **Create your own** 在 **If This Then That** 选择 This
    1. **Choose a service** 搜索框（Search services）中输入 「RSS」 -> 选择 RSS Feed
    1. **Choose trigger** 选择 New feed item
    1. **Complete trigger fields** Feed URL 中输入，资讯网站的 RSS URL，也就是本例子中的 https://cn.nytimes.com/rss/。点击 Create trigger
1. 在 **If This Then That** 选择 That
    1. **Choose action service** 搜索框（Search services）中输入 「Instapaper」 -> 选择 Instapaper
    1. IFTTT 若未绑定过 Instapaper，此步骤可能要进行绑定先 （可选）
    1. **Choose action** 选择 Save item
    1. **Complete action fields** 这里可以用默认，点击 Create action
1. **Review and finish** 点击 Finish，完成触发器的制作。

这样，之后资讯网站一有更新，你就能第一时间在 Instapaper 中收到文章推送了。

## 下面推荐一些资讯源：

**新闻**
- 端传媒：http://feeds.initium.news/theinitium
- 纽约时报中文网：https://cn.nytimes.com/rss/

**资讯**
- 中国数字时代：https://chinadigitaltimes.net/chinese/feed/

更多的优秀的资讯，可以自己去找，RSS 查找方法参考「寻找网站的 RSS 链接」。

## 进阶玩法

会了上面的之后，就可以自己研究一下进阶玩法，如

- Instapaper 支持推送至 Kindle 的服务
- 用 IFTTT 保存 Instapaper 中喜欢的文章和 Highlight 至 Evernote

有兴趣的继续摸索吧。

[RSS]: https://zh.wikipedia.org/wiki/RSS
[IFTTT]: https://ifttt.com/
[instapaper]: https://instapaper.com/
[instapaper_pinterest]: https://blog.instapaper.com/post/149374303661
[Instapaper_independent]: https://blog.instapaper.com/post/175953870856
[cn_nytimes]: https://cn.nytimes.com/
