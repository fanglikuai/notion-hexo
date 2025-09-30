---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTPUKETF%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T010037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCICXVA%2B2ZSXbrUYz0u0rByrqljINFJ%2FZrUNdO74e6dj%2FBAiA2f%2FaNUJ0aVqPPw0RMJhuKPAIqdnd6qU32xP%2FAB4TwWCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHUIc5okV8%2BUsdjnLKtwDi4qTkWb6AwbxK805ibqUcttC4o9UgceMXKfwshrpWaPT%2FmN150p%2FnXGY92gTJETXJuLA65jRc70hRSk5M4sMdGdZ2WwdmNm8aq4QLlH%2BKF%2FH1feforNwcL4UjSZEGcjgSvbJ4o5oC05g8wXPK2%2FGcNEgIsQnfGpmskoxT1KL%2BzJmd7hfJ%2BmESP0aU1FooaoyjglDJ2y%2FWHsecaesw8vckQ7bP7WBc7G48wb1gpwjdOfkuID0r9w1AUY%2B0L3Kl%2FWlzOPI5WJyFl4kgKddFGTDDWSdNLjGERfqiIZFK02bAdQCLGfB3mA6vJF4V3w%2Fp6xN9HQtg%2FfD4eQTpL7u2o%2FauTqn25wp%2B4oZIK1pLr3fYVeSkgYiWzlKTLLb9SyfEwFWDUcz%2BD5prznWJJbEZSa52MDYhD2gieUaCcdIL3holnCV2IWkP7W69OGwrj3kWaJc8YD4h0fkkP2Op%2FWG0Si7KJYEVB8nRMyv37GIzCj9mXszN3ZwzPO4UB5EcdTuPV6zZJEDUCPVvPCPTE1EwA4zjVfDNxpizvZSOpV2JzH57xIUnoa3ppS%2FBV0D7e3%2FPncOKd4j0iOmqKwNnQTMzFgGzZtE03If52g%2FWIBXks9LIKXkQS0B%2BdPtgae3Aqwwt8jsxgY6pgF5ykxfuI9U1JehHWoqHsEzxlXUI4ooSsNGCLFQ1%2FMz6LR9KIlzqMzIVbqCRsPZqT%2F7jqahwX8bfD2%2BF6rTVBiQlBryjVmTD4GbHwFfpBVeC0SI7NNBarfWNyAQOvZMkbnMdAgAHWQVnssctPRPCWYL%2BwdfLY%2FUyvZmfV%2BN2Wlza4tZXpE2jrD5FVnfgil%2BDbMBfHBuHL7aw%2FV4dy%2B5J7i4J0L7kqy2&X-Amz-Signature=3dc666797b3c7666b507c4648e60e5a4bdb35e7bcbc62ce91b49e1f106fcd51d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

