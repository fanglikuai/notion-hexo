---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFSMYXL6%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T120048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDsaCXVzLXdlc3QtMiJHMEUCIQC3CV3BU7bVGTuDi6RwAoGQfWnLdALt7N72m5kEv3hJgQIgVO7wWB6wKInPmDu2YXjtrgff%2BKp4cZoUZTk6NSQ9tiwq%2FwMIBBAAGgw2Mzc0MjMxODM4MDUiDGrowzDryHNnsKHeVSrcA%2Bajd%2Fw5tS3P4g%2FiMiBS5iKtC9nDi6%2FgsfJaIPg9joYXJW3xthIb%2FRFY%2B932Z58MhWUm29mkcLwObriAMpuxLMTJrZFkhxvX2WAbWQ%2BqeQtmeBF9s%2BWwOTthRLDFGPUbHw7Ot%2BjRxJsWSUoOF%2FXq9UDtVuVP9ljJFWqtdryy3pdjllYnZrPsi1pl9bZ%2BtqHMFTHmP1jhcGeKDg4R29X0yrjd8A3ce7cLABIbUZEdrsI9ObnUyz7oAbTsjIECDK5JOr5Vw7FxR1gjniyh1yfypw6ogsIqOISN7ihUdDa9VzWqoGZZn90yy4nPsPKrV4U0k5CSDke%2Bv1I8ub4gmjwhlR%2FYPdP3EKbG1UwkuDhC46IibS1dEg0t70a0rHqH7skTMSff91a%2B0I0fU4pQW5zRXbE%2FykR94tjg3QsEdaIRQq9Uw75S53MPey%2FxhEdepu8F30qEPtyk24TbrWvFb8zdnb6ALuAnzUhQp1liGtwmFUStexn1OX5Q6L1NJCO5OtNnmlW2T2SjSixWyIGNxnDgxcr4kKl%2Fu004bI%2BncBA6RBvIWzJ2UTE6wa3t4IYAlteTVb5Upne8dAyYEJdURVFdX5x2ZNP5NJgsSdrfT5mHq6Yw9sjJjKXtXW0PhIC6MI2Mx8gGOqUBRzbzC0iEmMOHAcu6w8Il%2FLmniA8jNkW%2FI4jX54oc9fR4Ujfiy57o3r9u%2F2Z41ufmyA1B9PeK71XON1fHpmgmkQFuJJ%2Fxq5dFi0ywh3hFX4GVskCyerVMR7HVuIaNAk8gN0zv0ctDFvOyAa3VVQIgBAGgL8kb%2FtRK1T4cDbGoOHkUcyZ0dkkuNOyB2TExf%2BqLH2pqWVisXlUGRdlp4R%2FE%2Bor1KsXF&X-Amz-Signature=d556947be22ecbb816e1ad179051d4210ee921df22b34876da6ef5ef9dda2fae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

