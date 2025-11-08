---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVH57MM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQChWiDYVX2Fus9lcbLQFO1eMAvMQhfhM6bjWfYfZ4JRGAIhAIOWCVOQY4snrhnutUO4%2FVx0NaW66MnlQdGoM08%2FzqCaKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyEDtGVp4nti21zpOEq3AMZ09Cp4%2FWn6rno7HTS7%2Bx5gici5oxB8KlEy%2BpsTguOD12qHKpyoQ7qBl%2BT1ldy7NNgFziZQDTtIIo5pFxGdx%2BScrS0SvgQQpusjbbGD1QPJYQF4JS6nkwHapDwnu4QfY2wq%2BkXkcG4fkZ1lqBprHdd6iyHiJvuQw5HnlDOKZZSm3zWToenj97SCI3ndaCfEMNyn5zTqvLckgho%2BXVsXKIqWKpMQYrjIvGz224j03MSpH9B%2F4aLWuBEhHtbsPFIvw7piO1Nz1pvfbFxzlCvZqvA%2ByfogCcArZsHNyPXi%2BBIUjMx3StYPwJAVIl1a5H6FueVMQpXfyGz7oMnKGdaiadmhXK0bI6E7h100geD%2F2OF0Kp0JMdS4ftdV%2Fqu3rUFJ17OtSw1btD2vppysJLS8zuu4OGHbX3rOTbYkj5hRC99ObyLFvw8n0xM%2B%2FSxn0vkZFTSNzt%2BYAdtwnl%2FjIpu9tMSAx%2FV9LaT%2Fb5%2FFZkixpWZ58R9QAWr%2FR2ALpIve4Ffzct4LL6T3UBjt%2FLKkjz6Z1xboj%2BGVPhTue21cphBEKyNEs9BaNpNZHr%2BmdGurmjKtuTwK%2BylmmhhIqg%2BXODx7cHWUnRepWGSOr%2BH%2B4LS6fRlFeJdFRczgiaWPWFUxDD2%2FL3IBjqkAVRHE%2FdtcoWpOLSHTTFtj%2FVo4gD53BSUkf2qSSmtExXvkWQyHWEOCd8463QZHcTvHNO4hemF%2FO47fvpO2jsshF6XOH%2F5U1voZ3SqLjljL4pmqIMSbFopeAb2CeXmwbQcRlUlPS8z5Bluk57ALy9f4x4pcyUECqd8DvMVADjkHqXSy%2B6BH8EpWVspHttyV7UpWcbGjEo9HydLVOEXyoXDDJ4XyjQw&X-Amz-Signature=4620c09b47dcdd8188f7f768fc099ea7d92f0c501aacb1a37fc377397bcf0265&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

