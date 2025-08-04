---
title: "如何彻底解决 PC 外接声卡卡顿爆音 - 少数派"
source: "https://sspai.com/post/100623"
published: 2025-06-26
created: 2025-07-26
tags:
  - "clippings"
---

> 本文详细介绍了作者如何解决Windows电脑外接声卡卡顿爆音的问题。作者通过更新驱动、调整BIOS设置、关闭节电选项、使用特定软件检测延迟问题，并最终通过调整电源管理和处理器设置彻底解决了问题。文章还提到了一些常见但无效的解决方案，并强调了在Windows系统上使用外接声卡可能遇到的兼容性问题。

本篇文章将详细描述我历时三天，仔细研究、逐一排查，解决 Windows 电脑外接声卡时，时不时出现卡顿、爆音的问题的过程。

事情是这样的，最近买了监听音箱，就把好几年前的买的 UR24C 拿出来当常备设备了（平常只有在录音的时候会用到，而且有时候在软件里会掉驱动）。

![[images/Pasted image 20250726213528.webp]]

产品外观参考图

接上监听音箱后，果不其然那个小毛病又出现了。不管是在听歌还是在打开 DAW 操作的时候。虽然只卡大概 0.5 秒就能连回去，但有时候真的很影响心情。而且还没有规律，我也不知道什么情况下会卡一下。当然，这种音频问题大多发生在 PC 上，我相信接 Mac 不会存在这个问题。

![[images/Pasted image 20250726213539.webp]]

错误提示

我卸载声卡驱动，去官网重装了驱动（斯坦伯格 UR 系列的驱动最后更新是在 2024 年末）。

将 buffer size 调高，不起作用。声音卡顿、爆音依旧出现。

于是上网一搜，好家伙，很多人都有和我一样的问题。很多都是近几年出现的，新电脑、新系统上尤其容易出现。可能导致问题的乱七八糟的理由竟如此之多。随便列举几个吧。

- 系统版本是 Windows 11，需要装回 Windows 10；
- 连接线需要连接 USB 2.0，如果电脑上已经没有 USB 2.0，需要买个转换器试试；
- 需要设置电脑 BIOS；
- 需要禁用显卡音频。

就我光查到的就至少有十多条，于是我又上 [官网](https://sspai.com/link?target=https%3A%2F%2Fwww.yamaha.com.cn%2Ffaq%2Ffaqs%2F947) 看了下，就看到了更为搞笑的——就连手机离得太近都算卡顿爆音理由。

当然以上理由来源冗杂，不足为信。

我列举下真正必须要注意的事项：

- 将声卡驱动更新到最新；
- 更新到最新的 BIOS，在各厂商官网下载最新的驱动；
- 在 BIOS 中关闭 C-state 和 Speedstep；
- 将显卡驱动（比如 N 卡）更新到最新
- 用 5V DC 电源给声卡外部供电，避免电脑本身供电不足，尤其是笔记本电脑；
- 最好用 C to C 线连接电脑和声卡；
- 如果你的电脑 USB 口不支持 USB 3.0，则需要在声卡驱动中选择 2.0 模式；
- 将电脑的电源选项调整为高性能；
- 在设备管理器中选择「通用串行总线控制器」中把所有设备的「电源管理」选项中的节电选择关闭。

![[images/Pasted image 20250726213548.webp]]

修改位置截图

不过，我做完以上几项后依旧没有解决问题，还是会时不时卡一下。这时候我网上搜索，找到了一个叫「LantencyMon」的软件，这个软件才是测试电脑是否有延迟、卡顿的「最终检测标准」。

下载地址： [https://www.resplendence.com/latencymon](https://sspai.com/link?target=https%3A%2F%2Fwww.resplendence.com%2Flatencymon)

![[images/Pasted image 20250726213555.webp]]

如图所示，软件的结论是你的系统看起来不能处理实时的音频任务，声音可能会卡顿。并且一个叫 `ACPI.sys` 的进程，导致了这个问题。

## ACPI.sys

对于这个问题，我在油管上终于找到了一个解决方法，不过这个解决方法对于 PC 来说不是一劳永逸的，每次重启电脑后都需要重新操作一次。

在设备管理器中，禁用或卸载以下几个选项，不用担心，重启电脑后这些驱动就会被自动装回来。

- 电池——Microsoft ACPI 兼容的控制方法电池；
- 系统设备——ACPI 处理器聚合器；
- 系统设备——Intel(R) Innovation Platform Framework Generic Participant；
- 系统设备——Intel(R) Innovation Platform Framework Manager；
- 系统设备——Intel(R) Innovation Platform Framework Processor Participant；
- 系统设备——Intel(R) Management Engine Interface #1；
- 系统设备——Microsoft Windows Management Interface for ACPI。

这个时候再跑 LantencyMon， `ACPI.sys` 的问题终于解决了，不过又出现了新的问题，比如 `wdf01000.sys` ，也是导致延迟升高的元凶之一。

## 最终解决方案

还是借助于油管大神，我终于彻底解决了 Windows 电脑外接声卡卡顿爆音的问题，这个困扰我三天的问题终于被我攻克了！期间我还找了某宝的人，感觉某宝的人自己都不会，最后也是退款。

简单总结下 [这个 1 分钟的视频](https://sspai.com/link?target=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DFFH8u_283mM) ：

- [下载 PowerSettingsExplorer](https://sspai.com/link?target=https%3A%2F%2Fwww.mediafire.com%2Ffile%2Fwt37sbsejk7iepm%2FPowerSettingsExplorer.zip) ；
- 在软件中找到并关闭「处理器闲置降级阈值」和「处理器闲置升级阈值」；
- 打开「控制面板」- 「电源选项」，确保选择「高性能」模式；
- 打开「更改高级电源设置」；
- 将「硬盘」「睡眠」和「处理器电源管理」按照下图进行设置。

![[images/Pasted image 20250726213606.webp]]

![[images/Pasted image 20250726213616.webp]]

- 下载 [UnparkCPU](https://sspai.com/link?target=https%3A%2F%2Fcoderbag.com%2Fprogramming-c%2Fdisable-cpu-core-parking-utility) ；
- 将 Core parking index 拉到 100%；

![[images/Pasted image 20250726213622.webp]]

操作截图

这个时候再去测 LantencyMon，我终于彻底解决了问题，能愉快地听歌了。

这也是为什么想省心省力就直接上 Mac 的原因，要是一个小白碰到这种声卡卡顿爆音掉驱动的问题，不上全网全力查找资料，那就得一直忍受。而且不排除随着 Windows 系统的更新，还会有更多的 bug 和兼容性问题出现。

顺带说下斯坦伯格的 UR 系列声卡，作为品牌旗下的经典产品，虽然产品本身不是声音卡顿爆音的元凶，但至少在 Windows 上需要谨慎购买。

再次提醒：如果是 `ACPI.sys` 的问题，是每天都需要操作一遍。

补充

作为一个使用 UR12（和 UR24C 其实一回事）录歌的产品用户必须插一嘴。经过无数次测试确认，这个莫名卡顿然后恢复的问题 100% 和电源有关，是 Windows、CPU 和 PC（USB）三者在电源动态分配上的作用最终影响了声卡。尤其是如果 PC 是双显卡系统（典型就是我的手提电脑），就要再多一只鬼：必须确保没有在使用第二显卡的软件在同时运行。

其实播放还比较少发生卡顿，录音才是频繁。这问题在我这里最典型是什么呢：比如开个 Audition 录歌，声音稍大了一些电平表波动猛烈冲顶，就能突然卡掉，0.5 秒内是完全录不到然后还能看到录音的波形幅度从无到有的回升过程。

感谢作者发掘的方法，回头我也试试对录音有无帮助。