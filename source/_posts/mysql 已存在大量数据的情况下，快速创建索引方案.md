---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T6ODTMZI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T170042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQC8Z1wkaPK2iT2N4o%2FyikoZXUUVl%2FnLDtnAlA32a2tqEAIhAN7snG6DrMnQIj%2Fa%2F%2ByQcnvMZi%2Bww1vvIQcq3RBTCDodKv8DCCEQABoMNjM3NDIzMTgzODA1Igwv6GQ3o7%2Bz%2FfSe8ocq3AP98Qk6iCUakxT1hp5SFbR7w%2B5zLnjMMaY4JCCxgjOl7b0J5vCfngHZ2Gs3iwe2kgzS1ebEBOpefkolp3Zmn5ndwDd8FqdL69LMRRyHqei6ZoJRXPqa0XjOV4MfDNjqcVW%2BgcvN9E%2BhMbW800yBTPQM2JoUtOoeMzNU65cS2yfSlWFy1SgUTfjL01e%2B1YPmX4%2FlHwRhVtH2IxnIxldqNd13TjvtMrNkC%2BfpBWUQ9dsiRi0TxE%2Bx%2FNKNEfgT30fA7fzabzduV0zpnXiiImot0vSvSS%2BlFOyE0uXqCXTHOJMDp6xoPqFxg9SpNdBApH6jyZtnehrMJsqwx18nBeysupbFm4ed4KyWsvRSq%2FFKE4zdn7pETi%2FtXGdT5VlxhHtQA%2BHQGAm7nFU3%2FdrYifICDOnxWTYTsbQCFe0scZtr2N1F%2FPer4mj%2FcoXn7uOdCUhuW%2BDhnGCZNOHlsQ4MlzAiC8Cw7TOMjp%2FTPpuv0NZIkLyZJxG9umIpjknhmF1cWLmmEOBpe3exM7kk%2Bc0R05vr94MQTZqYMJ2I2qGuU6wsH%2BR2d20qY3FVRBkQ1aZuhlm22mMxJqY1Ug8NfzRqtEiwqC%2B1jZkC%2BHG9j8hu5L1GfBYWykVOhaaNhSE2ESqeRjDbwM3IBjqkAa4MVnJUoq9V2toPfn6zbp0ezkQ%2FL2B%2BcXv0vH04CCsYuLtVKfjQIYqlo%2FmDx%2BiKxZSO3tIAiuBYRpk3GhtP4WWWbfxmvYqJmvC0M%2Fo3Euitbxbw6qC5YpRNXDIwWozL8%2BcfM%2FwCfpMdwxI1G%2FbNXflCZOxZDqiooEr5bAKRjdelTPZlX5R4eIzrqdPAb89PvBSYEB2NOvhZabJVqnrCxzo1LtyU&X-Amz-Signature=cab3736882d7a1e1da6270432da15084147e56eab9c4e7d3471c9243d846053e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

