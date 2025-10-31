---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VK7ZBI6J%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T120045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIHJhlcgPdMQnUl5wx%2BxOBXCtwKLB5NFp%2BZjuBwxVP52pAiBMgwN3vwGuYE3vCuarepd%2Bfz0r32nEO3URBB2WE2KapCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMt4U3f1coVO5maE2OKtwDd1iVz59vBnGDenYkaAE8%2F5lP3Ss5Cv4BWX6P6Uucjjgb6YbPxuKLaBBgtueZPPL%2FCH8J%2BB5CgAljBDsnG2so7iXh6BUlb5riSiXd7acLDB6vdPH45SS7iIxl%2Ft1z%2BL%2FYuDbRaf8ireXAIUXPMPI%2BkV5l5PrOBqkJP5A4%2FC3Dw%2FU8cOr4uByjqu1vHu53rhfdAhUW2Ae1d3as2D5rlZgN%2Bs6SdIXmIuqGCAc0REWNymXn4XgDAVKNFyjaQHcicJVZi5xVdDxT0edD6Hz6F%2BsKj3VH3tHGaqhrm9%2BnlThYV84izTr%2BCyE8P1oqd99dMfffyFmUnwkozlWGonXVcVmZ90pFx3hrGJDUTEd7qncPtnVEK46lmiWaohFHNRAjwCb%2BcA%2Fqh%2F8jY2lp48PdwFs%2Bo40dOArpAHgbKKNI6JIa%2FfTSqe1wvd%2FFCszfynTrulAcE7Htlr8yUb5ty2pH2tNw%2BEv9hlV%2BZe8CihKDgwoB%2FYTT%2BfXWMAR%2F1vWj1DKiSCeLVVvVRgQ1JBFJvlTqR10eZqsu6PwHH%2BC4zA3PUDGuVzGLxlD8sxIxGrT2Ux7dFs6XXgw%2BbnVrD91BSGwmuZSCu8%2FvKVaWvRgRtNUfq7JvR0Jh1V8ZLeIKXd0sQwgwobuSyAY6pgFbtXqLCqUyglOkcWd7SuPKgRBjxSRlPlZz4fFzp%2B0bBYhfClWWIFbBSWAy1w7DMUD9K%2Ftq9o3V9M7kmnrMgvwcgum9TxBRDSIMOgZWPe7vs6f6JqnzHQ6DYa0mUrq00V4%2BaPWg7lQM6BAJpgGFLLUdXKtcJRFFHEgtwVxgWXkwEWObwcxzDtUXHw%2FlS7ze1dsJv7cyBDCruwPdngo68fNUaQh9BL0E&X-Amz-Signature=4b42bb6b0bcdcc7eab66186bb1232b67a5aec207911900cea5bad2e4cadcf905&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

