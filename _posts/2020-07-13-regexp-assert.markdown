---
layout: post
title:  "正则表达式 先行断言(lookahead)/后行断言(lookbehind)"
date:   2020-07-13 21:15:38 +0800
categories: regexp
typora-root-url: ..
---
# (?=pattern) 正向先行断言
代表字符串中的一个位置，紧接该位置之后的字符序列能够匹配pattern。
例如对”a regular expression”这个字符串，要想匹配regular中的re，但不能匹配expression中的re，可以用”re(?=gular)”，该表达式限定了re右边的位置，这个位置之后是gular，但并不消耗gular这些字符，将表达式改为”re(?=gular).”，将会匹配reg，元字符.匹配了g，括号这一砣匹配了e和g之间的位置。

#### 例子

![不使用正向断言](/assets/2020/07/2020-07-14 08-29-31 的屏幕截图.png)
<center>不使用正向先行断言</center>

![使用正向断言](/assets/2020/07/2020-07-14 08-29-08 的屏幕截图.png)
<center>使用正向先行断言</center>

# (?!pattern) 负向先行断言
代表字符串中的一个位置，紧接该位置之后的字符序列不能匹配pattern。
例如对”regex represents regular expression”这个字符串，要想匹配除regex和regular之外的re，可以用”re(?!g)”，该表达式限定了re右边的位置，这个位置后面不是字符g。负向和正向的区别，就在于该位置之后的字符能否匹配括号中的表达式。

#### 例子

![使用正向断言](/assets/2020/07/2020-07-14 21-26-43 的屏幕截图.png)
<center>使用负向先行断言</center>


# (?<=pattern) 正向后行断言
代表字符串中的一个位置，紧接该位置之前的字符序列能够匹配pattern。
例如对”regex represents regular expression”这个字符串，有4个单词，要想匹配单词内部的re，但不匹配单词开头的re，可以用”(?<=\w)re”，单词内部的re，在re前面应该是一个单词字符。之所以叫后行断言，是因为正则表达式引擎在匹配字符串和表达式时，是从前向后逐个扫描字符串中的字符，并判断是否与表达式符合，当在表达式中遇到该断言时，正则表达式引擎需要往字符串前端检测已扫描过的字符，相对于扫描方向是向后的。

#### 例子

![使用正向断言](/assets/2020/07/2020-07-14 21-30-37 的屏幕截图.png)
<center>使用正向后行断言</center>

# (?<!pattern) 负向后行断言
代表字符串中的一个位置，紧接该位置之前的字符序列不能匹配pattern。
例如对”regex represents regular expression”这个字符串，要想匹配单词开头的re，可以用”(?<!\w)re”。单词开头的re，在本例中，也就是指不在单词内部的re，即re前面不是单词字符。当然也可以用”\bre”来匹配。


#### 例子

![使用正向断言](/assets/2020/07/2020-07-14 21-38-23 的屏幕截图.png)

<center>使用负向后行断言</center>

总之，带<的放前面，没有的放后面就行~