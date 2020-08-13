---
layout: post
title:  "golang plugin 包"
date:   2020-07-27 21:15:38 +0800
categories: golang
typora-root-url: ..
---

插件包,可以通过不重新编译代码内容从而达到添加新功能的一种形式，懒狗必备。

把一个库搞成一个插件包使用以下方式编译即可

```shell
go build -buildmode=plugin
```

随便写了点

```go
package main

import "log"

var Test02 = "???"

func Test01() {
	log.Print("??")
}

```

使用上面写的编译命令编译下来就是一个so后缀的文件

#### 调用

其实总共就两个方法，一个是Open,载入插件，以及Lookup调用对应方法或者变量，返回的是一个interface，有反射那味了。

```go
package main

import (
	"log"
	"plugin"
)

func main() {
	p, err := plugin.Open("test01.so")
	if err != nil {
		log.Println(err)
		return
	}
	f, err := p.Lookup("Test01")
	if err != nil {
		log.Println(err)
		return
	}
	f.(func())()
	v, err := p.Lookup("Test02")
	if err != nil {
		log.Println(err)
		return
	}
	log.Println(*v.(*string))

}
```

稍微需要注意的一点就是：var下的值是以指针形式返回的。

### 怎么用

看到这玩意的第一反应：这插件绝壁要用interface规范，然后就能爽了。

于是试试

```go
package pd

type PluginInterface interface{
	Test01()
	Test02(f interface{})
}

```

首先搞个interface


```go
package main

import (
	"log"
	"mytest/pd"
)

func GetInstance() pd.PluginInterface {
	return &TestStruct{}
}

type TestStruct struct {
}

func (o *TestStruct) Test01() {
	log.Print("??")
}

func (o *TestStruct) Test02(f interface{}) {
	log.Print(f)
}
```

插件包就通过GetInstance方法获取实例


```go
package main

import (
	"log"
	"mytest/pd"
	"plugin"
)

func main() {
	p, err := plugin.Open("test01.so")
	if err != nil {
		log.Println(err)
		return
	}
	s, err := p.Lookup("GetInstance")
	if err != nil {
		log.Println(err)
		return
	}
	i := s.(func()pd.PluginInterface)()
	i.Test01()
	i.Test02("sdfsdfsdf")
}
```

在main方法中直接把plugin搞出来的东西当作interface处理，突出一个爽。
