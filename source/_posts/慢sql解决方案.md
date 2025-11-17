---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WOOPCLKX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn0M5trbXl5Ye5RqKyIr0921Cxbzlmfi8znaHav8oC4gIgWrXT%2F1wtDSTtjhcajn7UUkp0GAsmfl5T4ARCgP4iubwqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL6zT%2BDxVwFWsqLDbyrcA6UcA7QDstEiDC572QjeP24kA5O8MXGmpzFcnUVco7XpHzeId8S2QWKn3H8bKhczRBFumBMaWV2C6w5l5StlwS1gidX1oVFDBoA8xJiOYpINa7RvtphJFPuo9JwQQ49Sis0Buqruz1LrB%2FS6lQ2FwD0lrDpv8Wnc8QFR2%2FFagexsvg7utCBPTooYPEUObv3z0l%2BxoKzI6K4C4bb%2BDAoJa2eYL2%2BIORWXWehjHgUsa01oBL%2FAQYdPzXqppW1A7sfwoRfkyX97yqrHOPIn8Otgv0mRe1cv%2Bxhq2pGPaIV8j8hyyVDd65uNO2E1MqS7pmUC2VyXz8WaeZV6js5u82rtRzDpWkRvxdw5LrwAysBoIfKH8KXwmOoInfj%2BHlrxqx0rSn1uHy3NxStHdSBb6CeCoEItV59C2q4ZDSYonpgDRmYrrbI%2F55n0szjIz6LXbCafPk8QO%2BnlU4gPtAc4f%2FQplpUZsqF5azjR1DlchhVTnr3SGLPoV%2FvGDQomLcRYHCORSX301e1rBAPZY9OeerNqzcdSHXruaoDEue3U2%2FnRMrMgrn%2BKCGA%2FugF5gLHhYdvDz4tZTocoCU7nUEbiO364MHyay%2FZOwoWHloPMWci8aLYEicQRUiuQ%2FY9ivr1qMKGm7cgGOqUBHht7RCJdRH9hFAiUaOoLlIKm9mRSxHnw3kDd8O0Bl5dfg48%2BsGHO%2BmjJesy8vdt%2BDSY7t4QQFOusaTvLIFyWy%2FTXMbL%2BsmyJl8Ay3iawN9UZwO98d%2BPmpZcrzEXFzNkVa%2Fe%2FD0lCKT7F99e85Zr6VAjl%2B561O%2F4tzYRxeCG3o927ScdT%2BOLqscqT1pJcXWxrYj0ID0AZb3SrqsOU9TelXzeDAaMD&X-Amz-Signature=24cf0a0d4c5d2132ecfc93bdcefbd4f2a8833a35e704da5c5550fb318e67bcee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

