---
layout: post
title:  "redis 01 string"
date:   2020-12-06 21:15:38 +0800
categories: redis string
typora-root-url: ..
---

# 全局

## EXISTS

是否存在

# String类型

## SET

NX 标签 对已存在的值设置将会设置失败

## MSET

一次性设置多个标签

## MSETNX

一次设置多个标签，一个有，所有的都会失败

## MGET GET

获取

### 字符串操作

#### STRLEN

查看字符串长度

#### GETRANGE

字符串截取 两边均为闭区间

#### SETTRANCE

字符串截取替换 ，闭区间 

#### APPEND

追加

### 数字操作

#### INCR INCRBY INCRBYFLOAT

增加

#### DECR DECRBY

减少