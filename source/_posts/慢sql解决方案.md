---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4XIZ35G%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHajrup75yOxHNoFFBK7Tzutvys%2B9QVGkGQIho9ABUHEAiAz4hMv4g7nnbq5oRkDJFLY6KMWo1ZGR8946Ns5KCxW5yqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTktgDAEedjAVmirJKtwDpePfm4A313CFh6S582rOoZAWre2z7gYZWV%2BQWnZ1Syd8oN%2BC%2FX822TwDOTXMXW6RKJJj45YXbHiuh0O%2F%2FYUT1tp3vbkD669hnkMRR6Xx9WsDgppTXplTAPUpE7PQUiSGGbvVg9nnCmDaHQ5%2BWdebBljtydyaAz%2Bx3xltj1diWVIRcpVgjH05Wo2yvxjlczUvBI2C4D%2FafFYungKyU9qkBmyYiWyGZTA8mE6VmqjiXnJiWS2Y5aRAXM12r%2FKkaoioQGcezBKHTlrIXDznVHSV4%2FqZ3OCbUx%2BKDZbZJiUYKZiAknlKPyS9QZ8Hq1xjYfW1rO36%2BVf3m8948FfnPKCIs6%2Bg4vpdNpkZsOgXfZUYrFFti4iMjv92qgRahcv%2F3kKENyAOsSPgQA57dUeVvPi%2F%2FbsKPnW6rJi8L5STZ5qvnjE7hgUyk%2FxBeAEfEz3TrTQE8nRuD8DqP%2B1WDYfetT7W5MFipstAMgWPJhqsItWruNGXW43JWNZnXd2zQN%2Fc6PybDfsTVbbpvgNHoD3LrFxY%2BSRxK4mW62IJcdHB94%2FiaSZ3bs6UPuVhQZ1qdhgm3Rdgp6N7Ubl%2BCbDFTHP%2BDz40qIWhIo2ewSJQqHerh6NFAo4%2Fu5cw0T3A1pmMF1MwzZyJyAY6pgF3K5UftpWKGuJdYu5CHFBC6bOTK0zWegBmrj3Jlfso%2FrHBO6XWzNFV%2FlQf2wmMc8zt4Ffewe9L7lUQkPDsrkxMZs0Q%2Bwo3WF5C6UdXanRxTkAkGoqE96xRPgBrDNR7FMQDjmpdOszVjc86lqzzGbhgI4RaJik7cNFTRQA4wBmqSMFw0tmKeer6ERE25CZjuo1VBkfwn5pJm8NXpoort1Q46cFynzc%2B&X-Amz-Signature=ca01c04bf0ac2e7a5a628c353575ebc0605da09902adb529dd4215ac762cc952&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

