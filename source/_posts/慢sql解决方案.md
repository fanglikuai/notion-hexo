---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAE4FH2A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFOe4RIEDA1DAOI5RdFiHjoOTdOY2iyZEqYhGMg4jZghAiBQrCgfd%2BOGLT5PZCCl0ZhOfG67vCJOuMQE2epnHT8R5SqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJ1H7n%2F65QDNeAA06KtwDf5f96yCu6LnRAqjCq2SfORvD2huqSRxX4OQkzzv0iYs28%2BPxP5UhLIFyfA%2FOW5Y9NQXfEa%2B%2B00I2xu4fBVHz4AHT%2BWSwH9Lu3mUTuMAyic4DiGVEyEDC59Bc9p6g2wFSi7Xmz1SsRqrT8jqJ95wCVF7KH%2BBivSm8PuIwb%2BbdzpoZrD4TK%2F7InGA3WsX8ZimEaJP24hIGVjTgofaHVa89ltYwVepi52kDahX5Iz%2FawXshovXWvaai8yCWNbeXwFGuYXZOvB%2FaQefY6akH14eTGElc%2F3kS4Ci9dTdUdrLh1A89WPhxECR6iqNYm%2FaNYbubLSI9IR05vCfqSkSYvK37OsL1%2BCypYIT%2Bc%2Bqece1ns3Md9P4D066ggeevu8pEEbaFXxfn%2BsWd7aG5xsD6FMZZ4G9MzK4lcCrcPdGlrAtYCpPL1pRbXc3GOC%2FB86ogvUay%2BYP6fnrDBubRnNCwgv1HdEuksBQGyrgxyXNKFTawlmDImTm250WwtRuS3gnGIuynvREt%2BKgypStNu%2FhHOM4JLAN08sL4IgXBkDwWpVf8%2BbKVkGbtyrXkMeYn9d5eklhBTUeBE4ZJz1agUsORNX%2BHKGPnq57ipoyLiPsbp1anyRj28OHoTi4jhCyUdE0wnZ7BxwY6pgFvlya5N3xoKjiKRBMignRze8NX1RJZwbSUH%2BpZA0Cm4VWP2DXgvZbLgewkRn2Y78SKUAZEsrmuiWqmNnztpZQlt%2BVi2BErPefLwhz9%2Fve69E1XBNIwaWAhR9MKkwkSO3sS45O4Tty8g8BTrVbqNjxaXuI2mGK25pGe1n6qFO%2ByJYUuFXsJHsn0aHraGCHpgTZUlMEqq4QkhS96%2FvsNXH1PSd%2FOhge9&X-Amz-Signature=bdf85c787f1cedbbd86f6d111d0f1b4eb587eadb0033db58ed879b0ebc29c108&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

