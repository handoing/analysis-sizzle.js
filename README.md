# analysis-sizzle.js

----------

sizzle.js source analysis.

Reference [http://www.cnblogs.com/aaronjs/](http://www.cnblogs.com/aaronjs/)

### Sizzle匹配原理： ###
1. 浏览器原生支持的方法，效率肯定比Sizzle自己js写的方法要高，优先使用也能保证Sizzle更高的工作效率，在不支持querySelectorAll方法的情况下，Sizzle也是优先判断是不是可以直接使用getElementById、getElementsByTag、getElementsByClassName等方法解决问题。

2. 相对复杂的情况，Sizzle总是选择先尽可能利用原生方法来查询选择来缩小待选范围，然后才会利用前面介绍的“编译原理”来对待选范围的元素逐个匹配筛选。进入到“编译”这个环节的工作流程有些复杂，效率相比前面的方法肯定会稍低一些，但Sizzle在努力尽量少用这些方法，同时也努力让给这些方法处理的结果集尽量小和简单，以便获得更高的效率。

3. 即便进入到这个“编译”的流程，Sizzle还做了缓存机制。Sizzle.compile是“编译”入口，也就是它会调用第三个核心方法superMatcher，compile方法将根据selector生成的匹配函数缓存起来了。还不止如此，tokenize方法，它其实也将根据selector做的分词结果缓存起来了。也就是说，当我们执行过一次Sizzle (selector)方法以后，下次再直接调用Sizzle (selector)方法，它内部最耗性能的“编译”过程不会再耗太多性能了，直接取之前缓存的方法就可以了。所谓“编译”的最大好处之一可能也就是便于缓存。