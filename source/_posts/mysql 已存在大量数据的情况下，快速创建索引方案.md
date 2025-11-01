---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROXWW4ZL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQCg54wyZkn62eOKr8LQr4LtQknFo28tBDLxKibj%2FQoY6wIgfKBWrqJCeCD6k3llKGjmPaG1PN%2Fl%2BpVYTG8T9b9z2Dkq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDBOyxsY9qUpTFFYSnircA%2Fq%2BkfnWhwJf%2BStu27XdbVyNznN2udm7hPFiMY6xUwmD1V5jy5zDExro86OQg9e%2BLRLNNm440OdIAuvbZmuIr5%2FY0bbMv1BUBPQJLjLeyrTcQWNFC5bKHYdPeAAnRM34V3TAJVXIL4k3YGq9%2FVW%2BW4oRnlk5nT%2F9tBxm46SCVST%2B86TDyV0p1rDxkMyRooC0%2F9Hct2AweheKMb7Bk%2FJ91T%2B3M54aP4yM2x%2F6LOsfTs%2FXhexDW0unYPblcNX3TUWcH4pR2fONiP7conMMAwLkp5DyscprIMuS2%2FQrpMeBZMv%2B8gZZv7k4nCafa2z6z8U4d2O4lzJeujCVn7VzktOo2LHrcNa0cxzHplWTZQu2F6Dh9FSXeKZ4TrV1ynUntFEEMBjYttx0rBb33o6zBN2xnVhHOTQw%2Fd9IBnOEqYVcNXyAgfCNwXJlALhJ0SKDnAAG6smQAiPSlYY5BOZuvhfnL5%2BHKbscTL0oLW3A50tuNlEH9PAZbdVOtaRqbJYKOd4q3lunHa7L20SBDc0LoHGVQE%2F8qoOOe899lRTC6xz64Aavn9rNzUYy%2Fr7Tg9JxmLBielizqWYqnGevG7X2DxROEXHMKUowsC8cnHpSn9YY%2FDdBhW4W7zjOEqHH%2F4fgMIDQlsgGOqUB6FM8b8BCPgLe%2BtnLPNTbiyNG3jk1esn9ms58ukP1LIrh%2BzDkdcPbSwaUlUPpvUcldDXlnlbWwE1KZiOMAEidGUxSFqJ89hPkNu3HtXb5oMIURocCGUzuSJEFHCS0BsiH2HQCVMEU8iHXXQprV0vCqfiYRDmcmH%2BpvKUfPgnhwl6obg8WRfIHREmVsjOvXIYrmM2twF4j5d6gF7bZd0MMMb7Lv2mL&X-Amz-Signature=6233f17de3477fe0c389997e8d82a0d8e3d3830219aea4fa44659475de5c91ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

