---
layout: post
title: 如何描述bug
category: 测试
summary:  
tags: [测试]
urlId: 20141216
---

{{ page.title }}
================

{{page.summary}}
	

##卖包子的故事
上大学的时候食堂里包子挺好吃，有好多种馅儿的。又一次和才哥去食堂吃饭。他负责去买包子我占座位，等了好久他还没有回来我就去面食窗口找他。发现好多人围观 突然听到买包子阿姨问“你吃什么馅儿的”（不知道说的哪里的方言，反正不好听懂），才哥回答 “我买包子”。阿姨问“你吃什么馅儿的”，才哥又回答“我买包子”。阿姨急了问“你到底吃什么馅儿的”，才哥很大声的回答“我买包子”。我赶紧过去说要四个韭菜馅儿的。一场血案可能就这样被制止了。后来问才哥才明白，他一直以为阿姨在问他买什么。

##论卖包子和测试共同点

很多时候有测试人员，跑过来说这个功能有bug,这个功能不行了。我说怎么不行了。他说一点就不行了。我说怎么不行了。他说就是不行。
这个不就演变成包子阿姨和我才哥的故事了吗。

##Bug我该如何对待你
我自己还是比较喜欢测试人员发现bug的。因为bug并不是代表我犯错了，相反我认为程序做的还不够好。但我比较讨厌的是糟糕的bug描述。泛泛而谈的标题，有的bug描述只有标题，错乱的步骤，夸大其词，杞人忧天，让人费解的方案。</br>

##Bug分析
＊标题 要简略</br>
＊指派 谁来处理这个问题</br>
＊重现步骤。问题再次出现相关步骤。</br>
＊优先级别。问题都紧迫性和重要性。</br>
＊严重程度。问题所产生的后果。</br>
＊解决方案</br>

##bug记录
1.标题要规范便于bug分类，后期bug查找。</br>
2.指派最好指派给一个团队，这样更容易跟踪和解决。</br>
3.重现步骤要详细而有简洁。开发人员更容易定位问题。</br>
4.优先级别。可以根据级别规定修复时间。</br>
5.严重程度。带来了那些影响。</br>
6.当问题关闭时要有相应当解决方案。后期出现可以直接参看。</br>


#总结
如果我们当bug报告易写易读。那样我们沟通成本解决问题的速度就会大大加快。出现问题并步可怕，可怕的是在最短时间内我们不能定位问题和解决问题。
