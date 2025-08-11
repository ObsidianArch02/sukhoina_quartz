---
title: 两天时间， AI 编程去魅 - V2EX
source: https://www.v2ex.com/t/1150141#reply7
published: 2025-08-05
created: 2025-08-11
tags:
  - clippings
---

> 该页面是V2EX论坛上的一个帖子，作者分享了自己开发一个基于Wi-Fi直连的文件传输工具的经历。作者最初因为微信和现有开源工具的不便，决定自己开发一个跨平台工具，使用Flutter、Kotlin和C++。然而，在开发过程中遇到了诸多挑战，尤其是Windows端的开发问题，最终决定将Windows部分单独用C#开发。帖子中还包含了其他用户的评论和讨论，涉及AI编程的局限性、跨平台开发的困难等。

**V2EX = way to explore** V2EX 是一个关于分享和探索的地方

上周五晚上，从电脑往手机发文件，感觉微信真难用，开源的 localsend 也非要在一个局域网（手机经常没连 wifi ），并且我魔法常驻（且虚拟网卡），和局域网发文件的软件都有点冲突。  
  
蠢人灵机一动，不如自己做一个用 wifi 直连的吧（就是类似于蓝牙的点对点，但比蓝牙快，又不需要任何一方开热点，连 wifi 网络之类的），且 wifi 直连在高版本的安卓和新的 win11 上才支持的比较好，刚好比较适合我这种喜欢用新软件的人。  
  
由于用了一点 flutter ，感觉这种写一次能生成多平台程序的真的好适合我这个懒人。而且最近用 cursor 编程太爽了，觉得自己超级行。  
  
梦是美好的，flutter 跨平台界面，kotlin 对付安卓 API ，cpp 调用 WinRT 。分工明确，架构完美🤣。和 GPT 唠了下嗑，它把我这想法吹的我都听爽了，而且觉得有 ai 编程，肯定秒秒钟做出来。  
  
然后开干，flutter 在安卓上确实没毛病，调用 kotlin 用安卓系统的 wifi 直连 api 也莫得问题，直到我开始弄 windows 这边。  
  
先是 cursor 犯傻，项目变大后，安卓上那边写的习惯，给我带到 cpp 这边来了，给我整乐了，而且 wifi 直连的 api 在安卓和 win 下面的设计都不一样，安卓下面还有什么群组概念，win 就是点对点。即使后端是 claude4 ，这 ai 也是反复犯傻，改它的 bug 比我手敲都吐血，一直在混淆两边的概念。  
  
写 bug ，包的！说完全对 ai 去魅，其实也没有，就是感觉现在 ai 上下文长了后也太菜了，记不住事，然后就是有些时候莫名其妙的幻觉，你说他明明知道那里有文件，他也可以读，他就是不去，假定后瞎写。  
  
然后就是 WinRT 的调用要用 cpp 或者 c#，和 flutter 一起用的时候，调试难的爆。Cmake 也不好折腾。ai 他自己编译，不过，看问题，瞎改，编译不过，上下文越来越长，操作越来越逆天，甚至从 github 上乱拉东西到项目目录来。我甚至担心他一个 rm -rf 把我扬了（开玩笑 rm 禁用了的）。  
  
本来计划周五一下午完事，结果周六下午都没折腾出来，一跺脚，直接把 win 部分来出来单开，用 C#（ WinUI)。想的只要协议规划一样的，能用就行。  
  
最后周末弄完初版，自己用着还行，速度也快。而且论原理的话，wifi 直连上限应该还是挺高的（起码比现在用 wifi 热点的上限高）。  
  
flutter:  
[https://github.com/jingcjie/WDCable\_flutter](https://github.com/jingcjie/WDCable_flutter)  
windows:  
[https://github.com/jingcjie/WDCableWUI](https://github.com/jingcjie/WDCableWUI)  
  
反正就是可以用 wifi 直连发消息，发文件，测网卡上限。  
  
然后想起来多语言支持都没做🤣,还没加中文进去，然后安卓端我最喜欢的夜间模式也没弄，下个周末再扯吧。下次不敢灵机一动了😭。

23 条回复 **•** 2025-08-07 01:25:53 +08:00

|  |  | 1  **[sentinelK](https://www.v2ex.com/member/sentinelK)** 4 天前  所以楼主提供了合理的上下文么？ claude sonnet 4 的上下文长度是 200K token 。 |
| --- | --- | --- |

|  |  | 2  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [sentinelK](https://www.v2ex.com/member/sentinelK) 我如果明用指令给他涉及的文件，比如说这个函数的调用是哪个文件，响应在哪个文件，他会调 mcp 去读就还好。   如果不说，他自己也可以索引的到，但是可能是错的。主要要是我把上下文所有文件理清楚了，也很消耗使用者的时间精力。   我尽可能让每部分高聚合低耦合，但是 flutter+native 要涉及的上线文确实比正常情况感觉要多。 |
| --- | --- | --- |

|  |  | 3  **[thealert](https://www.v2ex.com/member/thealert)** 4 天前  这是局域网传输功能？不是有好多软件可以直接用么 |
| --- | --- | --- |

|  |  | 4  **[NickLee123](https://www.v2ex.com/member/NickLee123)** 4 天前  @ [thealert](https://www.v2ex.com/member/thealert) 乐趣在于折腾，自己造轮子 |
| --- | --- | --- |

|  |  | 5  **[guiling](https://www.v2ex.com/member/guiling)** 4 天前  不出意外 Readme 也是 Ai 写的，"your\_username"还没替换，哈哈 |
| --- | --- | --- |

|  |  | 6  **[nananqujava](https://www.v2ex.com/member/nananqujava)** 4 天前  现在大模型对 cpp 的支持就是不行, 不用祛魅, 发挥长处就好, 比如大模型写 Python 或者 TS 就是很强 |
| --- | --- | --- |

|  |  | 7  **[AlynxZhou](https://www.v2ex.com/member/AlynxZhou)** 4 天前  这就是为什么我买手机有一条刚需就是 USB 3.0 接口……大部分情况下我都可以找到一条可靠的 USB 线…… |
| --- | --- | --- |

|  |  | 8  **[zuosiruan](https://www.v2ex.com/member/zuosiruan)** 4 天前 via iPhone 1  web 前端 ai 无敌 |
| --- | --- | --- |

|  |  | 9  **[Skifary](https://www.v2ex.com/member/Skifary)** 4 天前  最近很少看到吹 cursor 的帖子，是因为广告费用完了吗🐶 |
| --- | --- | --- |

|  |  | 10  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [thealert](https://www.v2ex.com/member/thealert) wifi 直连，就是 wifi 点对点连接，不是局域网 |
| --- | --- | --- |

|  |  | 11  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [AlynxZhou](https://www.v2ex.com/member/AlynxZhou) 嗯，我也会有线觉得靠谱点 |
| --- | --- | --- |

|  |  | 12  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [NickLee123](https://www.v2ex.com/member/NickLee123) 没有啦，你去找 wifi 直连项目都没几个现在，而且硬件要求那么高 |
| --- | --- | --- |

|  |  | 13  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [guiling](https://www.v2ex.com/member/guiling) 好尴尬🤣🤣🤣🤣，秒改了 |
| --- | --- | --- |

|  |  | 14  **[thealert](https://www.v2ex.com/member/thealert)** 4 天前  @ [jingcjie](https://www.v2ex.com/member/jingcjie) #10 了解，这个实际传输速度咋样，会被局域网传输速度快很多还是差不太多 |
| --- | --- | --- |

|  |  | 15  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前 via Android 1  @ [thealert](https://www.v2ex.com/member/thealert) 看网卡，好的网卡可以 100m 吧   我电脑是鸡哥，很差，和手机能跑 50m |
| --- | --- | --- |

|  |  | 16  **[livin2](https://www.v2ex.com/member/livin2)** 4 天前  图挺好看的，用哪个 AI 做的? |
| --- | --- | --- |

|  |  | 17  **[Dlad](https://www.v2ex.com/member/Dlad)** 4 天前  哈哈哈，删我数据库好几次。   但还是可用的——我完全不会 flutter 也通过 claude code 写了个跨平台 app ，准备上架了。      扬长避短吧。 |
| --- | --- | --- |

|  |  | 18  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前 via Android  @ [livin2](https://www.v2ex.com/member/livin2) gemini |
| --- | --- | --- |

|  |  | 19  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前 via Android  @ [Dlad](https://www.v2ex.com/member/Dlad) 在不写原生的时候 flutter 挺好的，要写 cpp 的时候我是感觉体验太差了 |
| --- | --- | --- |

|  |  | 20  **[TerryBlues](https://www.v2ex.com/member/TerryBlues)** 4 天前  写 CPP 就是不太行……同样的提示词做一个小工具，Python/PyQt 基本上一次完成，CPP/Qt 写出来基本不可用 [https://raw.githubusercontent.com/guozhigq/emoji\_storage/main/coolapk/coolapk\_emotion\_33\_wulian.png](https://raw.githubusercontent.com/guozhigq/emoji_storage/main/coolapk/coolapk_emotion_33_wulian.png) |
| --- | --- | --- |

|  |  | 21  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  4 天前  @ [TerryBlues](https://www.v2ex.com/member/TerryBlues) 对，一个就是现在 ai 对 cpp 本来就菜一点，另一个就是 flutter 的 cpp 和 dart 端交互感觉不行，有些错误太难 debug ，在编译的时候没出问题，然后这些 ai 自己也很难定位问题。 |
| --- | --- | --- |

|  |  | 22  **[NE555](https://www.v2ex.com/member/NE555)** 4 天前 via Android  看了下分享的 repo ，电脑端是需要安装 windows 版本的软件吗 |
| --- | --- | --- |

|  |  | 23  **[jingcjie](https://www.v2ex.com/member/jingcjie)**  OP  3 天前  @ [NE555](https://www.v2ex.com/member/NE555) 对，本来想做成一版的，但是 win 和安卓系统 api 差别比较大，确实没整出来。 |
| --- | --- | --- |

**[关于](https://www.v2ex.com/about) · [帮助文档](https://www.v2ex.com/help) · [自助推广系统](https://www.v2ex.com/pro/about) · [博客](https://blog.v2ex.com/) · [API](https://www.v2ex.com/help/api) · [FAQ](https://www.v2ex.com/faq) · [实用小工具](https://www.v2ex.com/tools) · 2680 人在线** 最高记录 6679 · [Select Language](https://www.v2ex.com/select/language) 创意工作者们的社区 World is powered by solitude VERSION: 3.9.8.5 · 21ms · [UTC 08:57](https://www.v2ex.com/worldclock#utc) · [PVG 16:57](https://www.v2ex.com/worldclock#pvg) · [LAX 01:57](https://www.v2ex.com/worldclock#lax) · [JFK 04:57](https://www.v2ex.com/worldclock#jfk)  
Developed with [CodeLauncher](https://cl.v2ex.pro/)  
♥ Do have faith in what you're doing.