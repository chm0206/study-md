# DFA算法实现原理

&emsp;&emsp;敏感词、文字过滤是一个网站必不可少的功能，如何设计一个好的、高效的过滤算法是非常有必要的。前段时间我一个朋友（马上毕业，接触编程不久）要我帮他看一个文字过滤的东西，它说检索效率非常慢。我把它程序拿过来一看，整个过程如下：读取敏感词库、如果HashSet集合中，获取页面上传文字，然后进行匹配。我就想这个过程肯定是非常慢的。对于他这个没有接触的人来说我想也只能想到这个，更高级点就是正则表达式。但是非常遗憾，这两种方法都是不可行的。当然，在我意识里没有我也没有认知到那个算法可以解决问题，但是Google知道！  

## DFA简介
&emsp;&emsp;在实现文字过滤的算法中，DFA是唯一比较好的实现算法。DFA即Deterministic Finite Automaton，也就是确定有穷自动机，它是是通过event和当前的state得到下一个state，即event+state=nextstate。下图展示了其状态的转换  

&emsp;&emsp;在这幅图中大写字母（S、U、V、Q）都是状态，小写字母a、b为动作。通过上图我们可以看到如下关系
![](https://img-blog.csdn.net/20140525154027187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

&emsp;&emsp;在实现敏感词过滤的算法中，我们必须要减少运算，而DFA在DFA算法中几乎没有什么计算，有的只是状态的转换。  

&emsp;&emsp;参考文献：http://www.iteye.com/topic/336577


##Java实现DFA算法实现敏感词过滤
&emsp;&emsp;在Java中实现敏感词过滤的关键就是DFA算法的实现。首先我们对上图进行剖析。在这过程中我们认为下面这种结构会更加清晰明了。  
![](https://img-blog.csdn.net/20140525154009593?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

&emsp;&emsp;同时这里没有状态转换，没有动作，有的只是Query（查找）。我们可以认为，通过S query U、V，通过U query V、P，通过V query U P。通过这样的转变我们可以将状态的转换转变为使用Java集合的查找。  

&emsp;&emsp;诚然，加入在我们的敏感词库中存在如下几个敏感词：日本人、日本鬼子、毛.泽.东。那么我需要构建成一个什么样的结构呢？  

&emsp;&emsp;首先：query 日 ---> {本}、query 本 --->{人、鬼子}、query 人 --->{null}、query 鬼 ---> {子}。形如下结构：  
![](https://img-blog.csdn.net/20140525153956218?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

&emsp;&emsp;下面我们在对这图进行扩展：  

![](https://img-blog.csdn.net/20140525153938078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

&emsp;&emsp;这样我们就将我们的敏感词库构建成了一个类似与一颗一颗的树，这样我们判断一个词是否为敏感词时就大大减少了检索的匹配范围。比如我们要判断日本人，根据第一个字我们就可以确认需要检索的是那棵树，然后再在这棵树中进行检索。  
&emsp;&emsp;但是如何来判断一个敏感词已经结束了呢？利用标识位来判断。  

&emsp;&emsp;所以对于这个关键是如何来构建一棵棵这样的敏感词树。下面我已Java中的HashMap为例来实现DFA算法。具体过程如下：  

`日本人，日本鬼子为例`

- 1、在hashMap中查询“日”看其是否在hashMap中存在，如果不存在，则证明已“日”开头的敏感词还不存在，则我们直接构建这样的一棵树。跳至3。

- 2、如果在hashMap中查找到了，表明存在以“日”开头的敏感词，设置hashMap = hashMap.get("日")，跳至1，依次匹配“本”、“人”。

- 3、判断该字是否为该词中的最后一个字。若是表示敏感词结束，设置标志位isEnd = 1，否则设置标志位isEnd = 0；

![](https://img-blog.csdn.net/20140525153918906?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
