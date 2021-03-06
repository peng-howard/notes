---
layout: post
title: (转载) 时间序列的建模与分析
categories: [计量经济学]
tags: [time series, VAR, VEC, 平稳, 单位根, 协整, ADF检验, Johansen协整检验]
---

原文地址：<http://yuanjiaoban.blog.sohu.com/45553379.html>

最近看过不少关于时间序列分析的文章，相当一部分对时间序列的建模和分析过程比较混乱，我个人的看法总结如下：

![时间序列分析建模流程](https://cssdpq.bn1.livefilestore.com/y2p7DzoEtmOeWBUeqczfYgV9JxPlaX6-y5dyAmXxhgkKp-IpZkID7q1IPMj4D46vhiQtQjuxtu_F1PuZcI3aR2IOcho5Ob-G3cyuHiayXoC4N4/%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E5%88%86%E6%9E%90%E5%BB%BA%E6%A8%A1%E6%B5%81%E7%A8%8B.jpg?psid=1)

补充：在VAR或VEC建模之后，还应该进行根模检验，如果检验通过，下面的脉冲响应和方差分解才稳健可靠。

<hr />

1. 不平稳数据也可以做VAR，只是需要协整检验，如果存在协整关系就不需要做差分使序列平稳了。
1. 如果没有协整关系，则需要对原始序列做差分(差分序列有经济含义更好)，使序列平稳后，再进行VAR估计。
    - 检查协整，在两个变量的情况下，用Engle-Granger method和Johansen或者Stock and Watson方法是完全一样的，但是在多个变量的情况下，最好不要用Engle-Granger的方法，直接检查Johansen方法中回归出来的矩阵的rank, 如果满秩，则所有的变量都为稳定的序列，直接使用VAR，如果是0秩，则所有的序列都进行一阶差分之后VAR(前提应该是全部的序列都是I(1))。
	- 本人根据<http://bbs.pinggu.org/thread-729486-1-1.html>和<http://bbs.cenet.org.cn/dispbbs.asp?boardid=33751&id=89051>总结认为，协整时用VEC，检验直接用Johansen方法是比较合理的步骤。
1. 脉冲响应和方差分析的话要求变量是平稳。
1. 考虑滞后阶的问题。
1. Granger因果检验中滞后阶确定时要考虑到自相关问题(可参考孙敬水的初级教程)。
1. VAR模型待估参数非常多，需要考虑SVAR模型。
1. 在一本书(还是网站)上曾经见过：Granger在提到协整这个问题时，说大部分序列都是非平稳的，因此直接做回归基本是伪回归，于是台下一片哗然，然后他又补充说其实没关系，大部分序列都是协整的，因此伪回归问题并没有现在大部分教材上强调的那么夸张。

<hr />
<http://yuanjiaoban.blog.sohu.com/45799173.html>“对向量自回归模型(VAR)的认识”：

> 传统的联立方程组的结构性方法是用经济理论来建立变量之间关系的模型。但是，经济理论通常并不足以对变量之间的动态联系提供一个严密的说明。并且，内生变量既可以出现在等式的左端又可以出现在等式的右端使得估计和推断更加复杂。Sims(1980) 提出了使用模型中的所有当期变量对所有变量的若干滞后变量进行回归，用于相关时间序列系统的预测和分析随机扰动对变量系统的动态影响，这是一种非结构化的多方程模型。它不带有任何事先约束条件，将每个变量均视为内生变量，避开了结构建模方法中需要对系统中每个内生变量关于所有变量滞后值函数的建模问题，它突出的一个核心问题是“让数据自己说话”(Gujarati, 1997)。 

> 因此，VAR模型本身不带有事先约束条件的特点，表明这个模型不需要所谓的理论论证，因为任何论证过程将违背“让数据自己说话”的基本原则，可是，目前很多重要的学术杂志上所发表的时间序列问题的文章，都试图在运用VAR模型分析之前先推导出一个向VAR靠拢的理论模型，这个过程真的有必要吗？

本人理解：**有必要**，原因在于即使随便找点毫不相关的数据来做，也可以看到数值上的结果，但是显然这样做是没有意义的。因此“让数据自己说话”应该是建立在一定内涵关系上的“自我驱动”。