---
title: "编程字体"
description: "常见编程等宽字体显示效果测试"
date: 2021-09-25T11:16:36+08:00
lastmod: 2026-09-11T20:52:37+08:00
math: false
categories:
    - 编程
tags:
    - font
comments: false
draft: false
build:
    list: always
---

## 编程字体的关注点

- 等宽

  必要条件。

- 区分易混淆字符

  必要条件。

- 是否支持连字(ligatures)

  有观点认为连字影响了字符整体宽度，在出版等环境是弊大于利的。

  个人只有视觉需求。

  另外有些字体下多个符号组成的组合符号，因为不同字符的重心高度不同，会导致整体符号（例如：*=）纵向高度不一致，非常影响视觉体验。

  当前主流编辑器基本都支持开启/关闭连字功能，因此字体本身提供连字支持是更好的选择。

- 中英混合排版是否对齐(中英字符宽度比是否 2:1)

  对于中国程序员来说，中英混排是很常见的。能够中英对齐更好，但也不算是刚需。

  一些旧的合成字体（如：consolas + 微软雅黑）无法做到 2:1 比例，但近年推出的合成字体基本已经能够实现了（可能在某些特定应用、特定字号未达到完美）。

  正常情况下中英文字符宽度比例大约是 5:3，实现中英文 2:1 宽度比例的方式有 2 种：

  1. 保留中文字符宽度，压缩英文字符宽度。这种实现方式导致英文字符视觉上偏瘦，好处是显示密度高
  2. 保留英文字符宽度，拉长中文字符间距。这种实现方式导致中文字符之间的空白较大，中文显得稀疏，整体显示密度低
  
  个人倾向于第一种方式。

## 测试环境

- 操作系统：windows 11
- 编辑器：sublime text 4，默认暗色主题，配色方案 `MonokaiEasyForRetina`

## 字体展示

**说明：**

1. 以下按字体名称排序

1. 英文字体显示中文时一般会 fallback 到操作系统默认的中文字体，理论上 windows 11 会调用 "微软雅黑" 字体显示。但可能是 sublime text 字体渲染的原因，以下图中各英文字体的中文显示部分看起来不像是完全由 "微软雅黑" 显示，比如 "将" 字的字型跟 "微软雅黑" 有出入（sublime text 在页签中显示中文文件名时存在类似问题）

1. 各编程用字体基本上在 "等宽"、"区分易混淆字符" 这两点上都满足要求，因此评价中不再赘述

1. 代码片段只是展示字体效果，请忽略内容正确性

1. 评价仅为个人主观意见

### 英文字体

#### Cascadia Code

Microsoft 官方出品，Microsoft Terminal 默认字体，字重相对较粗。

> 官方还推出了一款名为 [Cascadia Next](https://github.com/microsoft/cascadia-code/releases/tag/cascadia-next) 的 CJK 字体的预览版，截止当前已经预览 2 年了，不知道该字体是否还在继续开发。

**显示效果：**

![cascadia code](cascadia-code.png)

**评价：**

1. 中英宽度比非 2:1

1. 不太喜欢小写字母 `a`、`u` 的小尾巴

#### Code New Roman

**显示效果：**

![code new roman](code-new-roman.png)

**评价：**

1. 不支持连字

1. 中英宽度比非 2:1

1. `()` 的弧度有点过大了，成对的小括号几乎闭合成一个圆

1. 字体文件的 metadata 似乎并未区分斜体和粗体，导致识别可能有点问题

#### Courier Prime Code

**显示效果：**

![courier prime code](courier-prime-code.png)

**评价：**

1. 不支持连字

1. 中英宽度比非 2:1

1. 字符间距挺宽的，显得不拥挤

1. 挺不错的字体，除了 mono 字体外还有 sans 和 serif 两种普通字体

#### Fantasque Sans Mono

**显示效果：**

![fantasque sans mono](fantasque-sans-mono.png)

**评价：**

1. 中英宽度比非 2:1
2. 字体稍微有点非正统的风格，比如小写字母 `m`、`n` 最后一笔的小尾巴。看惯了一板一眼风格的字符后可以换一下体验
3. 自带了两种不同风格字母 `k` 的字体文件，除了这里的标准风格的 `k`，还有一种类似手写体的带圆圈的 `k`

#### Fira Code

非常著名且使用广泛的字体，连字支持非常完善。有其他字体专门从 Fira Code 中提取连字设计。

**显示效果：**

![fira code](fira-code.png)

**评价：**

1. 中英宽度比非 2:1
2. 不是很喜欢字符 `@` 的风格
3. 小写字母 `r` 是 Fira Code 标志性字符，是否喜欢则见仁见智
4. 图中实际使用字体是 Fira Code Retina，虽然也没看出来和标准版本有什么区别

#### Hack

**显示效果：**

![hack](hack.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. `i`、`l` 的向右的小尾巴挺好的
4. 中规中矩的字体，挺好的

#### Hermit

**显示效果：**

![hermit](hermit.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. `i`、`j` 上的圆点感觉有点重
4. 这个字体也是稍有一点另类的字体，偶尔可以换一换口味

#### IBM Plex Mono

**显示效果：**

![IBM plex mono](IBM-plex-mono.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. `i`、`f` 最下方竟然都是完整横线，可以竞争最丑 `if`
4. 斜体的 `t`、`i` 的小尾巴竟然几乎一样，`x` 又突然有点跳脱的感觉
5. IBM 出品，总感觉一种古板的气息，总之不是我的喜好

#### Inconsolata

**显示效果：**

![inconsolata](inconsolata.png)

**评价：**

1. 不支持连字
2. 虽然没看到明确声明，但从显示上看，中英宽度比 2:1，混合排版是可以对齐的
3. 中英宽度 2:1 说明这款字体是偏瘦的
4. 本字体除标准的 Light、Bold 等多种字重外，还提供了 Condensed、SemiCondensed、ExtraCondensed、UltraCondensed、Expanded、SemiExpanded、ExtraExpanded、UltraExpanded 等多种版本，但这些版本混合中文后中文似乎调用了宋体显示，很奇怪。完全不喜欢 windows 下的宋体显示，用的话就老老实实用标准版

#### Intel One Mono

**显示效果：**

![intel one mono](intel-one-mono.png)

**评价：**

1. 支持连字，但默认未开启，在 sublime text 中启用需要在 font.options 中增加 ss01
2. 中英宽度比非 2:1
3. Intel 出品，很有复古风的字体，挺不错

#### Iosevka

**显示效果：**

![iosevka](iosevka.png)

**评价：**

1. 中英宽度比 2:1，混合排版是可以对齐的。英文字符偏瘦
2. 如果需要中英混合排版，不如直接用"等距更纱黑体"

#### JetBrains Mono

**显示效果：**

![jetbrains mono](jetbrains-mono.png)

**评价：**

1. 支持连字，同时也提供了一个无连字版本
2. 中英宽度比非 2:1
3. 不大喜欢小写字母 `i` 最下边使用完整宽度的横线，显得过于厚重了。对于编程中常见的 `if` 关键字，`i` 的完整宽度横线使其显得比 `f` 还大，不好看

#### Menlo

MacOS 上的字体，在 windows 下效果只能说很一般。

**显示效果：**

![menlo](menlo.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. 带有"点"的字符竟然还是方的，如：`.`、`:`、`;` 等

#### Monaspace Argon

**显示效果：**

![monaspace argon](monaspace-argon.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. 小写字母 `i`、`l` 非常漂亮，`t` 持保留态度

#### Monego

MacOS 下著名的 Monaco 字体的开源修改版本，补齐了 Monaco 中缺少的**粗体**和*斜体*。

**显示效果：**

![monego](monego.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. `()`、`[]`、`{}` 间隔都有点小
4. 跟 Monaco 一样属于偏饱满的字体

#### Monoid

**显示效果：**

![monoid](monoid.png)

**评价：**

1. 连字的支持很有限，很多常用的连字都不支持，如 `>=`
2. 中英宽度比非 2:1，而且中文字符比例非常小，字重非常细，中英混合排版变得非常奇怪，属于**不可用**状态

#### Mononoki

**显示效果：**

![mononoki](mononoki.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. 实心的 `@` 实在是不好看
4. "点" 也是方的，无力吐槽
5. `()` 弧度过高

#### M+1 Code

**显示效果：**

![mplus1 code](mplus1code.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. M+ 家族的字体非常丰富，整体规规矩矩，挑不出什么毛病

#### Operator Mono

**显示效果：**

![operator mono](operator-mono.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1，中文字重非常细
3. `%` 竟然是实心的
4. 不大能接受手写风格的小写字母 `r`

#### Roboto Mono

**显示效果：**

![roboto mono](roboto-mono.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1
3. 小写字母 `i`、`j` 的点有点过小
4. 字体文件的 metadata 似乎并未区分斜体和粗体，导致识别可能有点问题

#### SF Mono

MacOS 新一代编程用字体。

**显示效果：**

![sf mono](sf-mono.png)

**评价：**

1. 不支持连字
2. 中英宽度比非 2:1，中文字重比较细
3. `@` 字符中间的圈几乎没有了
4. 在 windows 上的效果普普通通

#### Source Code Pro

Adobe 出品的著名开源字体。

**显示效果：**

![source code pro](source-code-pro.png)

**评价：**

1. 不支持连字，但开源社区提供了支持连字的修改字体 **Hasklig**
2. 中英宽度比非 2:1
3. 不愧是大厂出品，整体字型饱满，非常漂亮。字型饱满的小问题就是显示密度偏低

### 中英混合字体

#### JetBrains Maple Mono

由 JetBrains Mono 和 Maple Mono 字体合成。

**显示效果：**

![jetbrains maple mono](jetbrains-maple-mono.png)

**评价：**

1. 支持连字
2. 中英宽度比 2:1，实现方式是保留英文字符宽度，拉长中文字符间距

#### JetBrainsLxgwNerdMono

由 JetBrains Mono 和 霞鹜文楷等宽 字体合成，并添加了 Nerd 字体。

**显示效果：**

![JetBrainsLxgwNerdMono](jetbrains-lxgw-nerd-mono.png)

**评价：**

1. 不支持连字，有点奇怪，JetBrains Mono 本身是支持的
2. 中英宽度比 2:1，实现方式是保留英文字符宽度，拉长中文字符间距

#### LXGW Bright Code

这个字体是由英文的 Monaspace Argon 和中文的霞鹜文楷等宽合成的。

**显示效果：**

![lxgw bright code](lxgw-bright-code.png)

**评价：**

1. 不支持连字
2. 中英宽度比 2:1，字符宽度、间距都挺好

#### Maple Mono Normal

**显示效果：**

![maple mono](maple-mono.png)

**评价：**

1. 支持连字
2. 中英宽度比 2:1，实现方式是保留英文字符宽度，拉长中文字符间距
3. 英文是圆润的风格

#### Noto Sans Mono CJK SC

**显示效果：**

![noto sans mono](noto-sans-mono.png)

**评价：**

1. 支持连字
2. 中英宽度比 2:1，实现方式是保留中文字符宽度，压缩英文字符宽度
3. 这个字体在 sublime text 中似乎字符宽度计算有问题，导致显示错乱（参看图中表示空格的圆点的位置），使其在**编辑区**处于**不可用**状态（UI 区是可用的）

#### 等距更纱黑体 SC

**显示效果：**

![sarasa mono](sarasa-mono.png)

**评价：**

1. 不完全支持连字，常见的 `>=`、`->` 都未支持
2. 中英宽度比 2:1，实现方式是保留中文字符宽度，压缩英文字符宽度
3. 字体的 metadata 信息可能不满足 windows 规范要求，导致字体英文名称（Sarasa Mono SC）不识别，只能使用中文名

#### 霞鹜文楷等宽

**显示效果：**

![lxgw wenkai mono](lxgw-wenkai-mono.png)

**评价：**

1. 不支持连字，本字体的主要缺失
2. 中英宽度比 2:1，实现方式是保留中文字符宽度，压缩英文字符宽度
3. 霞鹜文楷 Regular 字体字重稍细，上图使用的实际是 Medium 字重
4. 字体的 metadata 信息可能不满足 windows 规范要求，Regular 字重支持英文名称（LXGW WenKai Mono），Medium 字重不支持，只能用中文名（霞鹜文楷等宽 Medium）
5. 中、英文的显示效果都非常好。中文的文楷非常漂亮，比正楷少装饰，字型更饱满，比黑体更有文艺范儿，不乏味；英文也很生动。**非常棒的字体，强烈推荐**

### 总结

现代化的编辑器应用支持多个字体配置（如：Visual Studio Code），当第一个指定的字体不支持当前字符时会自动依次 fallback 到下一个指定字体，直到找到支持该字符显示的字体。在这种编辑器中，可以首先选择自己喜欢的英文字体，后边再补充中文字体作为 fallback 字体，实现中英文由不同的字体显示（显示效果最好，但一般不能保证中英文 2:1 宽度比例）。

有些编辑器只能指定一种字体（如：sublime text），此时直接选择一种中英合成字体比较合适。我在 sublime text 中配置的字体是 `霞鹜文楷等宽 Medium`。

