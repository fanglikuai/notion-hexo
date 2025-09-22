---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RD2LLHZ2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDKGrLRalaOc3tj9ioh30c556tIHZfMV1812iOR7tkFOAiBkL2dtPj1ZHXi1JyYJiKhYSLdW1hYqMRbJSWxgYOZ4nyr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMY1GrHbpPh%2Fv7bDppKtwDuiOZrVqRiiAFvwimDaFGUGOsgCCsvFw7amCO6A7OserGIlE4dKXYEctYaxxG8vcx%2Bj7nPybglmkBBW63LSUeNRzh%2BshiGkYtn7mRTKqgFvRX60335ggwIHzMbGbEFecQ4NEgXdlkG6w%2FAE9o2g0crpx5ShAhfs94bVAB4fDKwMS3tQz1N39Sj3ie7rbqifjmx5kUlx52G05RYuN59uWgvf45Zvs3nylYJOiFhU5qDKa%2B0CwWOkkTa4jhK7th1M%2BpuyEOvSCBxQNWv5XbjPe4mVaOaFWdchzpogCg0AiUxE8G0YK8KfrVoaYU03Y8kHulHPC8%2Buo2n3OrOSR7D%2B5My8BNe7Ml1cFyBlNtlFkcbEuB%2B5hbu6KGzyNBRRE3s2m6wxv%2FGqdYKVEXYwOR6IEG9fPRsZHK0DLzB5SjYGIFmuaZ8JimuMsQFtqxNQAzDl5jY1hcOPhb0eXxvvy3HBfMMlZtO21HnET4ZDJj9kOKCC0wChKzkAgHnFYVIitlnpGPC%2BCycrgCEpODIecX21X7Vgx5hxjGzbtmpEvvBXKxWNC7k%2Fn9kNxoLLhGIKd0ui29plH10w8qE%2By6igPHeEW5H%2Bpyi54AsFxfLdKQZeoR0vmf1Rs1arBbHqcb%2FxIwn8rDxgY6pgEj%2BAwgZ5S4%2BIOaetcomcH9jqfgOeh1S2nNVVmEBXQgQtDfpBIr%2F2XBtY5PctZ7N3S7fz16jzkI33czR2MRNP0fx8PWrRrhgsnwIoUx18EzgBNMz%2FYSKyX29MefJ2WK2%2Bn6TDXbLczMy2pgBr75oVAus%2BAzjVThGTdT2YKRUty7V38y0JLzcprBCiMRJKUvaaBO6at1tNDH%2B4dwx3xt1LLNZ36OxLL1&X-Amz-Signature=94aeacf71b148a78ce8cc89924dcf9804d396e12e37e12284fd70bd7863068e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

