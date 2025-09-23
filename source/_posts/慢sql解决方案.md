---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAGIKH5A%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWKyhPT7uZwhA1lmcNvROsRV67lIrn3MRS%2FxeexA%2BJXQIhAK4KAWeQ%2FTirh8ujDrk9%2B9ov5p1DXTR2tW4l0kW6igqtKv8DCEQQABoMNjM3NDIzMTgzODA1IgzifE3mOHeMl61FQgoq3APFZZEd4Dvu6YPE0wx760StUedQSltT63Lf1cOBZ0InrYwNcHoIpwWvtpL%2BWhqY0sS%2BQ5zx2VyIbcOXfLKq0UOu1x2UV83iDFR4t3qN%2FWxnjWd0vw8bM8z2E8UrK6i6ffZZgHOT4QmxEPzjX6iC7ayEqTjPjZ5xRcQp4nquQnmKZXBE9JQSAJzgG22%2BS%2BursftR97X0fApb20iDoDImB94KSiazzZvgikKbF1e9c1TritldCHJf3seK0Ebtf%2Bs25QAqh%2B9npEctrCSbJ%2Bpzs6cOIeydVCK3PLQQ2xbKDJma0Z0KozexvVjhMMlDETCGu8lqfVqh6Si3G3V4o%2F2yrlzY9n4ali47O%2BNygvrOqmNBPEx1Yna76dFv1r74myG7wfjTLZbM4yKZc3CE9D3B4Igyedh4d9A8%2FRGdDASno50apIZtDexFUblyLcUC6%2FswmQ0RhFCTi9KTSJ33MsHKOZykjX2ee0TMLINo3YOcm9Z4g5xmHPoAQdrIcFAQYDDzqBb980ryUPIESe4%2F6UJKQ2Itr6rqv7MccAqkg7NjpZn5xGvHWRZHG5Grpfgz81p8sxq3uiJut%2B%2FEpRVw2pirZjHvItRXV8awlNq56zJqG8bLmStAtUzFwRxHlhJ1TjDG9snGBjqkAWeAQcCzOmbtbUNw6uIQSJLLhZeFNLoctFVfYaMrvcaDXO28EZQY92OpuvYiU7v%2FJRkZi2zx4nJZFY%2B1fAQZb%2BEGEHdpPtQmwI7Pf%2BbO1L3VQVM%2FGe6IE2%2BbJPOHXmUBdKJbZ%2F%2FbxKg55Nly3Cd6LaFb5PHs8DknY8RG6J6I%2BiMAehdMZEWKqNQNPeyWYlgwJSu68Utuoz0Z19IpUIzEqRrtl69Z&X-Amz-Signature=23e6dcc03bae18e701c51cc7b8bac5264dd5b632ac4a1f0410832934d6cf938a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

