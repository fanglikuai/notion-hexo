---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVQB5FCV%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWi4M7Z4YZk8R2fuidzS%2FCYTrEfiyvRs%2B9M8W%2ByVqO%2FQIhAPAFK%2FgcxtELVnN56EFaN4tKf0WA8F4VIIsf0oQHycVCKogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwjAPm5Vun2EsyG7Pwq3AMRT%2F4kjUZS4cBNunTJgfXilRqQa%2FLZH%2BveLTE4IVhnNdQACd7hmArfzH9M8l%2BuLz5SM%2FeJsOiHeMlULZZclLs2Hsz0Qld%2BLCQvOXUpjjGpJspdE9sbbgL44tGWsMdv%2FG%2BpHTAL2VPoduV%2F5hIlUAX6F1dlhmoTOJKd3iyVX7kfgAAJLyUWPtd3erU%2B0MjrQ5f4OBURvMgQWi2kNhC%2F4LFIbtBAKhizYhYPWAd7E0Fo5Jh%2BBrFCEwU8eLvTXeVFSP4z%2BRBq83gGcRgpTcXZehowZEPj%2FNbJuj8gmwMIZn2b8gmQ1BB%2FVCotLpxgS2EdFJWtADkGvRmirSAHkrnv3gMcK4ETptUW8YCRom6NhkiWbXkwwa2Sc%2F5abkLqRKndTWfuPElVYvTSWW2KP2bdp0eawRMe%2FLPfDg0NFBa2Za%2BW9MYYMjxtIjPIptvnymRadPik2aPQhlf%2BCgIHNEWx4xUEGd5RuI1%2Bcpl%2BWT%2FRqCcI9CGPP%2Fgii%2Bm35WMAqUdcGbtd2q0ez%2F3z9kvI22Xy9xhDdAC416oZheXKSgE5JvdV7nAmym7cxMykFtkqoFCJgFSBLNc8jqJgas%2Bd1cQKuRJtXZVbrKkmdLORdLEZozY2Sap9AeEqz2TeJx43NTD6%2F6%2FIBjqkAXgmf6E9B9DirtiG7VcHdNRR5RwebzMUzUU4U0CU6dMiOyNq%2BR7AJOsBSr9LjxyRNRSBs8U70cYCoWiZhSB2uj1YmQiiGwS3wWq9fmkK01ZeqiQUzEKwFa4Yh%2FQo4nvIbo9BVTMwIUiPqrEUZs9j0ePweWXtw8oQ7Bw9MF7%2BQbwWf%2FNnkhmo17agMYjUEjN083gEMBHyqSMouEtirUDTHQaNsvxB&X-Amz-Signature=b33f36adb47560f545e874e2075bbf9204580f9ba4984d096036e3f26d7b3627&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

