---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOVBHCGX%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T110128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIFDsgBZj8tJCRVP9QuUfLNP49Q3g3McDDVKBudMpIvArAiEAsuhWE1ZIcJE%2FL1lb8%2Fc5jEFMqIueFLSDuhgutaKTPCoqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMxV%2BW5omuWcMo79kircAxKV7udXGFQVdg2ApEzRKwGu9gLSw0Wqdw%2FY%2BUThlCaCSlyeN43z5kW6oKeGgiNfYdd4nuWhqWPjLOOB8sRu095axl22e9mZJoApDIMmLqD9XBIj6%2BB3CjvAVP7d0%2B%2FFMfVbVKZivi35%2Fje0jqepJA1X14gb3LWzhe77AwaDX4misPZeCUst8Rpdvvmh9j2sP%2FjawnTMgJ0oCb3ellmDI8e%2BfOZcDcOLGkeUKQuFgupNOiTL%2BcqJDztMAQEC5hjBdxvmiB8psbO%2Be0mWlgB5fafcJJfGuCs%2FhARyVgFNcJ0aAdY%2BaQu2CH64%2BR%2FlmoKpkIg7ZmSWAdJxgPuzsuaziqyll9dbea1mKVsBAHn15tWgUW9Q7z8EfZu61i%2F3mxWS1Vomku7MbHryxAJWOFvWkneIRydUH%2F%2Bvu8J4udDlRa3U6H2bF0%2Bdd98Ken8tvf3Q0aXnDUdvxhGuW2mEfJVpoqE%2FMwTETVSKnuXaCmNXWyFML2pkAwCjs0n9A%2FWFH8yfGd6tgtQwS0rLMXQ7vumMV6HrhjSXrcSPNpQz%2F%2BsiJ%2BctsB1jBOnRfUquu4hRISbBXgEnWkULmX15la%2BxLg5CFCCLvqFdyeCM3WSNcugJ0KuyHL8LBojPQa%2Flhwa3MPey6cYGOqUBV%2BPlYvAHKPqfv9N5F9YjnVrECggvLZBv%2BWeolXXskpDgUglmG27o5lJgTHWpTBPOOXN38WcLNNkDU6kD06e%2F77MVhPPmYFtyTtUcoJPoVAiSzMSPJCTDjYDh7Kew7XZzVPa3Tsbjkh6SuFEYmMdR0c57bU1%2BwC8OZfY0dxJQo8zDKcM3wwRA8Ewu%2F6BSGXMP%2FvcHsYtwLUbdeuRHjFW%2FzJMDEG6%2B&X-Amz-Signature=6e7abfbcce02936c9600fa4d82cc6bd167a40f1ae3b0d0419044ce5cd991c582&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

