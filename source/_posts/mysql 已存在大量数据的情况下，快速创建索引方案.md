---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM7O76AP%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDUx6AZKS9z2xAnX%2Fx0AoHLYUAXvkNWhcL%2BKl1SYpwfqQIgXW9cemByUR%2BgwdZM%2Fw71bxGvImW%2BlWi0iNJg6OLcGmAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJa5VLpKuFLvp7rWPCrcA0kY8aK6Fg3TXJpmt8GX1C0%2Fuu5kdRvm%2BUB5UZXARijQVsNyRVxsKokY01kVsJ9KSrRMUXTDaWaXN3JCGvc%2Bs%2BpyZfPpjPS63t1xo8uOxjj4nRVIuU6M55xUMo4Fh5VFLA8kJ2bWHX5fMyjXbV2oGfvS52pJEMf9klKANVsUAUO65TjnE6nuElOCTRznzqB3F4NAlyEPerEDOVbRqPZmxl%2BX7LqScrJNsLk3c9VJohaKnx9JSKW1XfYHU%2F804%2FZR8cHF98Dfr50kfCwKW3jv1nUimLVy%2F3UU5UYhggVH2qYg7uatIbUegMdgok9Riq%2B%2B6VpZ252aAhtYxdAiYOJuhEejXTEdMbcovnW%2F0IV5Odqm0345UqY0lU23iUfmDcOpTseH8Tnr%2BWCk17Ibab6k9t8HGuxC3HP9wpKkdci%2BTAdrVk1dSus0vt%2BHnvP5P7BobN58nHy3z0EgBE0dv8niJQ3Cg1DbVirtFhzeHftdp3IKtJ7upp5lphqvdgAeXD3B5x2dm1nzHxeN8SHX%2FITXu4%2FA9uAvHQ9XYxJLylKsqSQPhF5vQv3D534gOFInfDpmwN4hzT1yFx9L00FVE7VTh%2BJZIqnK5qJWXo%2FzB6deNjSp7%2FniSO1qYO3xbo9OMIfhy8cGOqUBpbWBABgey0OuQLVj3LixrC1LHbzF4oKpe8oAmV7Wbcn1Ct2n%2BmXAjNcPCOfnDNlZOPUjHf7UQnsfqui%2FC2Y%2BT3Y9SvLeja376n0KBwsXvKi0%2BykwcxI%2BSMBRuc43aevl8XQqxtvcqWSjQ2wZ9Mh8wPTeVLasqk3YWkYG%2FnMoHWAEgl6Ue1gSfw%2BKHiRpneowEPB%2B88ehUNdr09f4dtaDeEJDF7wA&X-Amz-Signature=87b4eb82a959d3be4c242432101af4029a54ef075a4ff08a26ac4b72891cbfbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

