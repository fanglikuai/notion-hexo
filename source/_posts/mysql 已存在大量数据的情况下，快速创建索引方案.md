---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFKWQK3H%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIF0CJIByZ%2BVEYx9U4DiE3KaOrdZWQkFb2%2F6Nm47Zd3GIAiA80Sa9%2FIhPvuKc7O%2B6W6QNP7V8uzziRGZgU22juu6URiqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5shLJsFXJiGsCoafKtwDOqg2Esd7NSFLu8ebuYgsEdiDA2mzBgAvdcMP4%2BbKfz7yukTqWsTVKe46l9HR1Uy0TDOHNuP2o%2Br2bxjbXffvaeRldMn73QZf%2FSV1OISTcWhbtFnEt3qr7NB00TTH07CqpIcCKrxPD8iAnk%2FzfL2Tpr5ihNE5LrxMZcO5QXIhxmKa7PadSNmlQTxJjMLimUQPbdYM2nG7GREvRMNDmEiin2AXuGLIhBetHxg1O6RETfMPNFKhmyZ%2FtbCKE9afXVkAhNR0d%2FLgevjfHH9lbjPjkOaRc2SvUayVrDvsPju9bbxCl%2B%2BOvf%2FiEZloDCO%2FcPpbrvnkGQjhZpfZ2AA9Fumhhr4JuArDzIAg7pCg671Dtkm%2FkMDGoeMtccTZROp%2FB2cX881iS6o0yK4Ll628UwLVIo0FjbV5WybR5XERoh%2B7nR0yZ85M5amPQiqGAgrjQMe7O87IsPH%2F8QzNu8GLpK3BPSWrPlRfu3aHfIK6wCFNU%2FYaNNYkHhwGFwRaoqFodCFz9dtvLQ6L88s9P9ZDOKOvUtuimmSr9IoCUPY0hV8jSRMK51deNWrOBtVWx3ovcCfpZHtr1UQ6goOa9Y4cRXsP%2FqBGOedu46pDJnuljHFL4TssiF1sHhbFff%2FZYzUww%2BfyyAY6pgElHiPvry0I%2B3MX3l7fhyJZhM%2FbQZ3wWj1JYQ0%2FM58nMM9jZQBG6TwheC%2F0lTRjgrlKPNoWqvadN5j2USp3hjFCmMosJ0D4hgzYx8Qld1oJqCHdHliG8s1HmkpxGgsTN4%2FUkNh8ntvBGrmsCQdtzZSX9qC9OptHmFeKEP0vvob00v2%2F8UBqTDpDL41x3tARM3K%2BQbSBUAK5ewVgZ6r2LpdJEEBJQIon&X-Amz-Signature=91971a32b262def744a288e6558eb1fe776ebb55eb1e237baeee324098f97925&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

