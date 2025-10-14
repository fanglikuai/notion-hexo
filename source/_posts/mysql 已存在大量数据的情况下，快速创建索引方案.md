---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQN25MRN%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T140124Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICCGucY0xLcT6ee5GMAP30WT%2BNdr5hF5t8uQeaJe%2BRUvAiB6%2FCod79gXbVDfWHN6Er18ceryoXX%2F%2FaaR9g8xX9Fbrir%2FAwheEAAaDDYzNzQyMzE4MzgwNSIMvhHQjuE%2BULhQNfA6KtwDnsOZq3ji8vRcIwYdCfRJdIt2Zy%2BTniWKcVfCDG%2BAlUl6GQCwnq2QnloMY1428TqLmXbPvDFLwCI6sr1glFCfB57qonf4zyNdCmhEdoly0kdENU7Z6jX14%2FcIvq%2BbliFJ%2FQh8bJdxI2GvobazYdr3YdzCH41dZFZAFOEF8D76Z3lwRQzw%2BYf2fKlJ6X8TYI0Kmj0GxORJGgfRfGhkpStwejsGTrIWR0ewzKIW%2F%2FCCZdfLmlzgIKjxr7sSECq1bUq1ACl71J2S1kWjoeUTxhDjqQokBSALevOB0OACHcgsxR5fFYBLn56zL4gyMWmJK%2F8rpVxMsXcuAHK%2BqjtQABJvuM%2BurCDtIyi2PpBlggjx0cpOzeE3K3CQn%2FVEC%2FNAyDVWs4Sag0IEP4KO8x30gtfpSAas%2F%2B8b8du0QAMPNOWPa9JsEIIvmnV6ROSdePjdyD%2BW9AqAPz0Yf31s9dompewLKW7J96lXAPmsZBtZOazkSF%2FJByyi2dtz7btr02%2F2hdzD65Kdv5vQHAzp7qE9LKJT%2BIhRUfhu6fAVxJKpdQHXh1ZCPX9yI08n%2FWleLwgAWtV0k4ZX1KAv%2Fqqu6XgkYqKnasqpe7w8mb6TEC%2BW8B6jXMCTHEOljI0iHtiCErwwz5a5xwY6pgH%2FqnJjUgS4QsQCMoK%2FrH4QZKMtropLJXTHDIBmU4fNdVRvb2xdfZmurZAkFQGDFrNL1n8UEx3npErc%2F0%2F2xogrZ0VQ%2BbdekMdeFRtxA1YxZVPnfa6ImZ1LhoFGnPSOJNmLpbpHgRp%2FVhdOFi9V6hBln9Yb8L0ZXiXwQUOWUhymKmsXJjzwhsjor6MnQpX6aY0PuuteJ2QTO4K5OJiBj%2B%2B01IazsMqA&X-Amz-Signature=2107f0c0e93bde57effb36bf02f843f28bb628a80c78895f711c137f052a0a08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

