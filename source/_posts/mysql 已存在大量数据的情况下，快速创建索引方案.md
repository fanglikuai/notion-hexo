---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZZLTGC%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAFyg%2BofLeBav%2BCIzCDtnlDLJHILj%2BnosPT%2FdTQV7BjoAiBzSD6mqmZWo4h6IjKjE1gIFZVyA1Qo%2BPECGTv8zcC%2BDCr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrgxru%2Fxxq5jA1kk2KtwD50IOA0egredVCBdmzF5scZgNQ867zUR4HUbtyp1c6lbqwOEr2Qba0wDD0i6iX2Q8whGl159KYFnfExG3Ieu80wN4aKCaZcPKKZwb14vEQB7K%2B9ncF%2Bsvul59Kx3rXLssi7WkRNa6%2BpWLOv89L2ljAmFmIyOxQPdUe9jxGdAvTS41DNIpR0Q0kxoujhflwA4C970LhC5CBG6zM2Goyv6ozUvtxjgraDBnWZPU5p74rrYuX0fV9UxID9dDmfgN7%2Fy2WCYQbI4yXnr5QGnA49dDfYTo4j5hx673LVM33%2Ffejulspm3MZVYcevVMwf8CmYGF65rw7SqSkEWuQknCsjYeL%2BcNaga7gnD0brN0WUYVRQsKveWy%2FG9rbbXNYVFBH7QjKpN6kbgmCjMKZn0gmBjWT46E9l0Ei8v5dkMLOwNf8rd5gnbzK5nfkoRzZsqEg4jEEqxQkBSOTb4bkSP6ocQM31AIO5qgN%2F72p2hKsFPqvpV2jtGSnjqAx43X7zUKILL10xpsyiKf%2BTWqMhITe%2FV2r9USqbGniVABzpA0c1GLj1W6qaHxHyxYD1hSY45POp3Lu%2Bjro%2Fa2Ps9H%2FcFRqE8QqNJ15ghTwQtb%2FkIkBDtga3aFGWRszeV9x96HeSYwqIm8xwY6pgGBKJMqtkrvMpRpUEbpOXeP4k0KjsP813KUjgvR22GD%2BzL6wSiSKnWaq86THtphcjotsPMMUw5Wpr6vEkoFu0%2FpVXWIPDqMqgCgehThp8pn7eleplIoxAaLwcs83%2Fe%2FIV6o5QZAm6W9FbzF72abicNzQi4FNrTgr7dXtJ%2Fwrf8mnKbewi8pna%2FKksHfqab43vQfgHUsiT7WG5YtrDKAwYo9FZX3gdRk&X-Amz-Signature=3413a01e9350f1fa010bb5d1ac33681da7769d7e2e37a581af0eb241be6c7eb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

