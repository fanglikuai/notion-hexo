---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GEDTNOM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJHMEUCIF8oXOhN1xTeLCv4JD%2BkRIfZzt8zlbJc8oWfFWW1%2BFY2AiEAvDakQ4dSz5viMTHe8rPMoSr%2B%2FoTVuAKh%2FBqChWfBpH8qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bp%2F6TQsyJSGpA0BCrcA3xq%2B7e41aYxILYPCadbLblAc3tO5oZY5%2FrWkfKeOwHPd2qEMjfQ9mPf8qYvtK5iobmvN2mF3DdxHfSBBafQxKowg7vO82Cr57IOlLxN5nAMcz8kv8nuMUbRWE40mWaSBO6LgSKcj4JJN9RU1nbCohpid%2B%2BVK7UvGMyxOLxeK4M%2BiFQgFUPe8fBNudXwJtuXvmdPDhqzcjQ96UQbT7bTnKIv1ukGfkfpkNSfyCVNIx0uxrUX6zTDxvb70G%2BREVom3hzRulIy9NwEJRDTZBXL%2FUSGPSrgraTzOIngURwYZVxOU22K52HcZZZfJ15gsnYsOSNlP1YVgHeLxMQs8QyZLcDY%2Bgjar9aDIriVld576O5zWusidBywQKyQ%2BGtcMwUx%2BqMltnIJoIke5WUGUPOvTtlpKY1OfrF5n0aETAdNfh6cm%2Fvr8n7ti4JVj2JJ7VpfCrO4LoQHL8f6ONTb6h7I27O2b8IZvxCyIN3EOE%2Ft%2F7QANsvYWt1mPCLu5ktymQsqJaayQpxmyGpXGcqhYSfOJtAFyBo7bToi1RM%2FhmHCYyYWIqZWxVCpkSc7O8%2BajKMxFkqSPQxIylWpmKxoNuiZ2LVGCxZPmH2ltNkHRo%2B2CsKPUadrTDKToOb%2BYuSnMKKTu8gGOqUBDE3%2BMopCmghHtn88XSMAHnhPlR%2FD9MQFHiZrdhxGjR9B7zpn2BN9qloKIfRdEbHym1UvA5s%2BILib11przRUngyfsO1h8nk%2BgeJMg81qAY6nNYyGGLUAbpAVlo4ZOvyCTNPXwOkzQG5yov1ods%2BpPNy9MF8y1oE0y8ffscjMllZiCV0uPQrD%2F9T0xfQuT96Q7YYTf3gpfekhEY8wfzBbWY3gNiMPN&X-Amz-Signature=b7537ff6d2473843bd66c05b84b44d1de34b18e51fc8b897b339c03f3a0ab2fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

