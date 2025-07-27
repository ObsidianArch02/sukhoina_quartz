---
title: "[rime配置] 请问如何设置打开某app始终切换到英文模式？ · Issue #164 · rime/squirrel"
source: "https://github.com/rime/squirrel/issues/164"
author:
  - "[[GitHub]]"
published:
created: 2024-12-05
description: "我看到Squirrel支持所谓静默模式，我理解应该是可以让我在打开某应用的时候始终是英文输入模式？ 但目前的情况是：在用iterm的时候如果偶尔我切换到中文模式，比如open一个中文名pdf，这时切换到pdf阅读。等过一会再切换回iterm窗口的时候，发觉iterm依然是中文模式，并未自动切换成英文模式。 请问，能否设置为只要切换到iterm窗口，该窗口的输入界面始终是英文模式？ 我目前的squirrel.custom.yaml里的相关配置如下： patch: app_..."
tags:
  - "clippings"
---

这个问题这么多年没有人回答，我来回答一下。目前本人只在MacOS下配置使用，而且能满足这种要求。具体如下：

用vim等文本编辑器打开`squirrel.custom.yaml`；  
具体配置：  
`"app\_options/com.runningwithcrayons.Alfred-3/ascii\_mode": true`  
`"app\_options/com.sublimetext.3/ascii\_mode": true`  
`"app\_options/com.google.Chrome/ascii\_mode": true`  
`"app\_options/org.gnu.Emacs/ascii\_mode": true`  
`"app\_options/com.googlecode.iterm2/ascii\_mode": true`  
以上文件怎么获取具体的path（仅限macOS）：  
a. 例如：Emacs，path：`/Application/Emacs.app/Contents`
b. 打开`info.plist`  
c. 搜索"BundleIdentifier"  
d. 获取其值：`org.gnu.Emacs`  
e.然后就有了上面的具体配置。