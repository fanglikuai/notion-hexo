---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQBYYAUJ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDz05JrteQzegC%2BUErK4rMSRdDg4ZAmBnpYiH3xMIA83AiAbgwjhhcnnCk%2Bh1%2FOrC06LKYeBHsNpIOGmU8F4QwjsrSr%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMqn1cPql3IDM%2FjiAfKtwDFNhv5R8y54JqkoyjCd8woluhauSafSVrtzFVc5iGV9hP7bQ2Dlz7huPSa0aSaZ9wsvHM3ybGqzDhKrgNmOmvQHzm68LYaAVRq4B%2BqQS8jjNwwhQPudH1WNOya0rrhQMfW%2FLYvhW14dJUzSC09GCFdFJ4RpGO3q43NjLsFB01b02i%2FIXG3dYj1NM6%2BgPKp%2Fe0Dm%2Bmi0kUiNRdopDFmKysLjrUn1U2DDWxmf88XVE%2Ft6%2F8BGgaJdSo4lFOrWWbjfl5MvCQyxj9wqAb6g8v1k1sHgagldSE2YlnkQEpGUrSLfutwgub7NSa2l3Rzf04R1pfjEHHGA0TeW7%2B53irkoscbN4R8ryxgX3%2FpuRsEThl0uxP02kmxGTfUnjNludMbY%2B84Uqe6UZ64nisWxwkoGBWXdiy8kecfKLzym5KI%2Bf9NR6P6WZZJCPwfXqdYG3vCSzAgpyj7d9CQa0j5ZfzrRRbQV%2BEubVgO77THg3vLQcodrp86%2Fdh5n0cBa59lmEUQOhHzhSdEqwB6y70UnsiQ1Dljfwj6W%2F%2FKVKivOyt0BgAJDMBHBa9AnZmfmqhp3YWd%2BGGRtw6q5gA2wXtCQgOMBaLPPemcfKRqqCrZO5MWqpzot6s2hdTU5yFRuO2Ixgw7pOfxgY6pgGPn9p7MEkNZ6C8WKs%2BeU%2FOBRhuU16MSgIDWfAE9er%2B3iH9KQSIyIAsnpfsdi5%2BDcYA8XitTgV%2BfV%2B5DEykffk5DR%2BHqiED9LSpmJeg90mnfaYQNow%2BPCYL5plu1c745eQ3gAyN%2F0aE6mYxe5jikoS62lHoQ2Og9sbXg%2F98e9E0DXlm%2BFrxQOSesw05nq2Ns2SPy7ycmq7A7SfrwRjsuaz%2B5IDd%2Ft2e&X-Amz-Signature=cdd2f94807e3f0e86f1d3386dbc413e80617353313d2c74e4890194b268f6977&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

