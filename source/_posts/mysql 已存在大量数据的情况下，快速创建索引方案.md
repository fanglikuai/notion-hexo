---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663W6UN4MR%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkwkmukkU3Axafh2b8HPMdj%2Fmx80TC841vSS8BkJ5izwIhAJoqRii7HbUsydNE%2FansFMoJl41TY0E3XXTyuJRTThZLKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDGbZ4D9ISt3KqFWcq3ANB8HZOckrUmrxicAOqJNhjSdiMIeMwghK5oTOjmZKrWCqIXhp8h9ffKBlUgnAOL5730aIJxVSd1IP1mXvrL8sEKIXl1jog3Kj2N4se8vtvzvZA4WrVyg1TrYy%2FPoOGjUJUU%2FyaNxkO2E8Q2sQIGBFQWOjZNnjN7rhTXiK8C4SBU8%2BlXHUxZfOTHFtGaPCEkUM%2BcFwPK8zWsZ4iG1JQmIEW549QbpIDAP96v%2FXI45kxnrk9uC2O37BbiR81p7Cmz5Ks3tHFNEbpG2BZFRrD97cOun0qEm8rARx%2Fk2tuXGlTGHGqnlm2U8KArMv1VWYZ7IO%2BS%2F2NRUux9cYGcf%2FzLAP9zy9TpQzIXyVYX%2FxKnHZVasFp37X7u2gGJMNt7gSNsbCr10opXsDNArMkzfEJLBM%2FgbKff8nJsPCMG525uR4fgNM1KVlMA7Gd7GwVlzjcFxRemleMX7F8LDQVopLEsv6Ene2laXpfH%2F1zVBsE99jodSWccTTnRoHciXrrkcujlYhjcIowTYxXeRe1litU6%2FzS%2BwL5A38Zj3%2FaynH266LIwR4l6x8WmLVyczU8dOuIYhRo7stdiGGWGuCAobTwVTX7eRl2KI508BeWQ0vvfJ2faUyfwNRCOxNj0mg8zjCGjsLHBjqkAdVJgOtt9K5T0wigEmQu9av7u2mvorGNkrscig6UcnQymNa5Qgacz%2BxW7svVq%2FwcYZ%2F6uCJtMwRiLRoR3YD3KddI4mptBBE%2BWSdq5RcbmHzqLrFwlXxv4LbMiIHottsIrb7eaLjzvZrqW0MCUNapDr1lxBrzvb5waYcpNkw%2B5a2Z84yBjeBlzvvZNaKQr0Rps4d6dJ5dLAVdDzAd195oR%2BPLYlf1&X-Amz-Signature=8aee44716da099bede26f9636fb434e4c2cad53a8c4150ac36272ba84fba438e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

