## 安全竞赛培训小结 - 2019年8月17日

### 0x00 前言

很久很久以前，我还蛮爱通过博客来记录自己的学习和生活。但，随着工作，生活的节奏加快，分享欲望的逐步降低，渐渐的变成了一片荒芜。

在29周岁生日的今天，听到了一句话，“只不过是从头再来罢了”。

所以，立个Flag，从今天起，重拾开源分享精神。

那么，开始预备工作：

1. 在GitHub上建个公共仓库。
2. 在家里的群晖上搭个自建图床。（基于Docker搭建Chevereto，[配置参考](http://www.nasyun.com/thread-64644-1-1.html) ）

### 0x01 密码篇

解题思路：答案一般为flag，key，CTF等开头，中间辅以花括号{}。

故我们可以简单的利用Python的ord和chr函数编写一些简单的转换函数，与上述字母比较数字大小来猜测凯撒密码的规律。

编程苦手这边推荐一个工具：米斯特安全团队  CTFCrackTools pro，该工具高分屏使用略辛苦。

凯撒密码例子：[变异的凯撒](http://www.shiyanbar.com/ctf/2038)

> 加密密文：afZ_r9VYfScOeO_UL^RWUc
> 格式：flag{ }

先转换 afZ_ 为 Ascii 码，可知 a:97, f:102, Z:90, _:95

再转换 flag 为 Ascii 码，可知 f:102, l:108, a:97, g:103

可发现一个等差数列，5，6，7，8。

故：flag{Caesar_variation}

栅栏密码例子：

> fghpl{epase}

穷举4种可能性。

[![7eb03bfc9541ca08bdb6bbbcbdc5536c.png](http://dyfree.synology.me:22338/images/2019/08/19/7eb03bfc9541ca08bdb6bbbcbdc5536c.png)](http://dyfree.synology.me:22338/image/AQh)

通过观察得出，第3栏应该是正确答案。

### 0x02 杂项篇

解题思路：基本都是脑洞清奇类的题目，很容易推荐思路只有肝。

隐写术例子：[当眼花的时候，会显示两张图](http://www.shiyanbar.com/ctf/44)

1. 由题目可猜，可能是两张图重叠，使用foremost拆包。

[![explorer_pvfYfnJZdR.md.png](http://dyfree.synology.me:22338/images/2019/08/19/explorer_pvfYfnJZdR.md.png)](http://dyfree.synology.me:22338/image/EzY)

2. 看起来图片貌似有点像，对比一下大小，发现后面一张图的占地面积稍微大于第一张图，猜测可能在图的某些通道藏了点数据。

[![BCompare_AVDeBimreO.md.jpg](http://dyfree.synology.me:22338/images/2019/08/19/BCompare_AVDeBimreO.md.jpg)](http://dyfree.synology.me:22338/image/9eD)

184个差异像素，目测离答案不远了。

3. 使用Stegsolve.jar的data extract依次查看一下低通道的信息，在Red(0)区找到了答案。

[![BCompare_fSoyTfLHfG.md.png](http://dyfree.synology.me:22338/images/2019/08/19/BCompare_fSoyTfLHfG.md.png)](http://dyfree.synology.me:22338/image/MdH)

> 基本全靠蒙系列。

### 0x03 Web篇

解题思路：这部分个人感觉主要需要有一定的网页编码经验，以及熟悉常用的网管工具。

流量包例子：[包](https://ctf.bugku.com/files/e0b57d15b3f8e6190e72987177da1ffd/key.pcapng)

1. 用WireShark打开流量包。
2. 使用基本过滤语法，优先保留http观察下情况。
3. 使用follow根据tcp stream。
4. 然后就找到答案了。=   =

[![Wireshark_EMwrJV8igU.md.png](http://dyfree.synology.me:22338/images/2019/08/19/Wireshark_EMwrJV8igU.md.png)](http://dyfree.synology.me:22338/image/PNw)



渗透例子：[简单的sql注入](http://www.shiyanbar.com/ctf/1875)

常用工具：[sqlmap](http://sqlmap.org/) 需要Python2。

自动注入：

python sqlmap.py -u "http://ctf5.shiyanbar.com/423/web/?id=1" --dbs

python sqlmap.py -u "http://ctf5.shiyanbar.com/423/web/?id=1" --random-agent --tamper "space2comment.py,space2plus.py,space2randomblank.py"  --dbms=mysql --dump -C flag -T flag -D web1

[手工注入1](https://blog.csdn.net/dongyanwen6036/article/details/77825479)

[手工注入2](https://www.jianshu.com/p/0089b2503655)





> 结束语：
>
> 授课人：玩Pwn是爷爷，玩Re是爸爸，玩Web只能当孙子。
>
> 入门建议：
>
> > 公共知识：
> > Linux基础、计算机组成原理、操作系统原理、网络协议分析
> >
> > **A方向：** IDA、OD等工具使用、逆向工程、密码学、缓冲区溢出
> >
> > A方向推荐书籍：
> > > - 汇编语言第3版-王爽
> > > - IDA Pro权威指南
> > > - 揭秘家庭路由器0day漏洞挖掘技术
> > > - RE for Beginners（逆向工程入门）
> > > - 自己动手写操作系统
> > > - 黑客攻防技术宝典：系统实战篇
> >
> > **B方向：**Web安全、网络安全、内网渗透、数据库安全等，OWASP 10的安全漏洞
> >
> > B方向推荐书籍：
> > > - Web安全深度剖析
> > > - Web应用安全权威指南
> > > - 白帽子讲Web安全
> > > - 黑客攻防技术宝典 Web实战篇
> > > - Web前端黑客技术揭秘
> > > - 黑客秘籍-渗透测试实用指南
> > > - 代码审计：企业级Web代码安全架构

