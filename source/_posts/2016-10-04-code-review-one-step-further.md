title: 让代码审查扮演更好的角色
date: 2016-10-04 23:17:23
category: Engineering
thumbnailImage: 1.jpg
---

代码审查（Code Review）是很多大公司里面都有的一个流程。它指的是一个人编码，另有几个人负责审查，并提出修改意见。代码审查在大多数情况下对公司整体的工程质量是有提高的，但是如果使用不当的话，很可能反倒会降低工程质量。代码审查究竟在一个组织里面是有正面效应或者是负面效应取决于很多因素，而我认为其中最重要的是代码审查在开发过程中扮演的角色。

<!-- more -->

{% asset_img 1.jpg Code Review %}

首先，我们先看看在代码审查中所需要找出的问题类型。它们可以是：

1. 语法及代码风格问题：一般有静态检查工具可以解决，但难免有疏漏。
2. 效率问题：需要有一定经验的人来辨别低效的部分。
3. 命名问题：这其实是一个很经常出现也很重要的问题。对于一个人来讲说得通的命名不见得对于团队而言说得通，所以很多时候较难的命名要由团队通过代码审查协同解决。
4. 设计问题：小到接口的设计，大到服务间通信的协议，都属于设计问题，根据情况可以由小部分人或者整个团队解决。设计问题是代码审查中最常见的问题。

对于前三种问题，相对来讲都很好解决。其中相对棘手的莫过效率问题，但实际上基本上知道效率问题的人都知道优化方案。然而，如果一个审查的人突然提出一个很合理的设计问题，需要你重新修改源代码，你会发现你需要花大量地时间重新编写。

例如，在编写一个JavaScript库packageA的时候，你提交了代码审查。有人可能会提醒你：packageA是用于桌面端网站的库，相对应的还有一个移动端的库packageB。为了保持工程上的一致性，建议把packageA改成盒packageB一样的API。一致性一直以来是一个让人无法反驳的设计追求，所以你只好把辛辛苦苦自己设计好的API全部重改…

所以，若你的代码里面被提出存在设计问题，消耗的工程时间会增加。而工程时间对公司来讲就是金钱。

造成存在需要大改的设计问题的原因其实无非三个：

1. 设计能力不足
2. 对开发的系统不熟悉，缺乏上下文（Context）
3. 过晚提交代码审查

前两个原因都很直白，但是第三个原因有点匪夷所思。什么叫做过晚提交代码审查？

我想是代码审查英文单词中的"Review"给予人的误导，很多人是在代码几乎完成或者已经完成后才提交代码审查的。就好像在做一盘菜，做到最后一步的时候才想起来要尝一小口看看味道对不对，结果发现没加盐。

在最后一步进行代码审查，还会因为审查者一下子接收太多信息，而造成他可能无法发现一些应该发现的问题。

{% asset_img 3.png Gas %}

显然“审查”扮演的角色在这里出现了问题，它不应该是传统意义上的到最后一步进行把关，而应该是贯穿整个编码过程的一个辅助过程。用比较老式的软件工程“土话”说，它应该是一个Umbrella Activity（雨伞活动），全程保护编码过程的质量。

现在，我的代码审查流程是这样的：首先完成一个基本的设计，加上基本的注释，达到一个完成度——最可能出现大设计问题的完成度。接着commit，并推入到代码审查中，邀请其他人来审查。这基本上就是对他们说，“看，这是我写的，很简单，可能烂得跟一坨屎一样，麻烦你们帮我看看有没有什么大问题”。

{% asset_img 2.png Gas %}

稍微有点开发经验的人，都可以大概估计出自己手头的工作进行到哪一步可能出现大的设计问题。例如，当你在设计一个新的模块，那么可能出现大的设计问题的时候可能就是设计API的时候。再紧接着，下一个可能出现大的设计问题的就是类之间的抽象关系，等等。

我甚至还会自己给自己的代码进行审查。这并不是在做验算，而是在通过代码审查告诉团队自己的疑问，提出自己的想法，这样大家就能更好地与你沟通。相信我，把有疑问、犹豫不决的地方提出来；有自己独特想法的地方，也要指出来，因为你的独特想法有时候对团队来讲就是不好的想法。

每当遇到心里觉得可能出现大的设计问题的时候，尽量利用代码审查，让团队和你一起解决。对于工程经验少的人来说（比如我），更应该多做一点这样的事。一开始这样做可能反倒会开销更多人的时间，但是过一阵子之后，你就更有把握做好的设计决策。换句话说，发生大设计问题的概率就会降低。因为你总能在和别人沟通的时候学到新东西。

然而，如果每次都在编码完成之后再进行代码审查，虽说最后经过代码审查可能也会产出高质量的代码，可你将花大部分时间在烦闷上，而花很少的时间真正体会他人提出的意见的真正价值。

长此以往，整个工程团队的工程时间可以得到显著的下降。首先是因为每个人的经验都能通过代码审查增长得更快，因此总体工程效率会提高；第二是因为全程保护的代码审查很好地解决（或缓解）各种层面的设计问题，让工程无论从短期还是长期来讲，需要花费的工程时间降低，并且技术债务（technical debt）也会减少。

幸运的是，虽说这里提到的是比较宏观的流程问题，却是一件落实到每个工程师自身的事情。也就是说，代码审查如何执行最终还是归结于编码的工程师个人。整个流程的转换无需有新的工具加入，也不需要有很多复杂的文档。所需要的只不过是对团队的一次培训——这篇文章或许就是一个不错的素材。