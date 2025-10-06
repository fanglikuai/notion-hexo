---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIV2EQK7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZ2vLUgFCDpncRsV13SuVnl1FkyEg4vFXYJ2NDBLWHRAiEAoAh9xKcpiMR9EzZdFhWxV20YYBQ0IzgbXSSmKx1rqOgqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2BlmowX2Z7kSjMaTCrcA5xS3KhzX%2FT1mzMZ4OtjPQwrJg2w2V7HM4%2FegGZ4b3AysC2kiXvel7T2etgSdvBnRIZrKJpc%2Bdky%2B%2FersRtN7r6gYJDjmQ00MnPePDB6%2BYvRqoiKRHpuYdTx67HjCy8JLrDNHX%2B7MADF79yfkSTCKDC1AtFgofUrZEMH6Q%2BxavBIym0CAwMqIIIL26FZuWbROnrwTc%2BmFUzfXga4TaCxYAIrNismZwPY1t3vDE7H5dBM7PeZtVMEq0WHPv7dhKN85vUGSc1Z%2B7e%2BIJ1we%2BK0dIbMd4ICcsBsY2sdIgoUL%2BkcjhJc7Wl%2FZ9TWjrwvipaxZFP%2F3N3F%2Bof8tmG7AdMqYUxmVWfvFdAerdC%2BpzXjnlnVR85QFVLccrRkB%2B08H6ACecddZ%2F%2B1u6peEgZBgEt5fPNi9Uul6viKrMlFXWdd66tCJ8PDysskrRjLdsTgwjt4%2F5OHMVB4CkP%2FOqyod1oMEZbH1%2BsYJXFeHbMSa%2By1zlxlrEEHL%2FGIaAmpGodyuZP01HjM%2F%2FELBsqbd%2BQK4wiRH3doH1hrUL8wjs655Zc7WZBeKz79viZsYIXBBfufdP7UKyIaUQC29kmUBkfASmDTt22xlgDp%2FCastZrvTZ8EqVONROlU0HC50Vk4rXPrMPb0kMcGOqUBP2tc1QtTTrYGUSgNdlZ2guif1JxreTBlAgLCDqjnOzAOkBlVeyqVC5e8a%2B9AcOWxlJ4%2BykyXNLlLcvF63ZA2soNV16QNO2z%2BpfIZ6ouZnDSlLSn9Dtiks4kTEScXB70r2DTN7eq346vxw%2BhtTrCdp4KtgrvfUmfuQ4E1NWHlVG2nO06tEPpGsuMrGzy90bpfwkn5dglfGPyVilH57aNSXYbWlreQ&X-Amz-Signature=047f9a9bf1d2b5d9203c6e2c0d0f55f0849227b1b5314de648e12f4000ab051e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

