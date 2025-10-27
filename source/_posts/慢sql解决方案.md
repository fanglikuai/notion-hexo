---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGZTP2Z7%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T220136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAbHKxsodESw4xdyahhnTlpSuB%2B1Qlw9zMnmfBQrWnJ6AiBSzjlcXLoAvHDJJBjyrpPwiC%2F86CO27ZkJl%2BOs8%2F2bpyqIBAiu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuRk9IIqIX0OzfNNSKtwDXD7KbGpPbcSCdMvL7MgEzt%2FSkvQ%2B5DmnhQBqOJIMA4gSgUTaDGmn7IkXrFiNEiPxGBCxiktZsSgikHiTcZRNlKK8P7SJ58nbiuVgPG6xn92w5tcvlK33r729KE3E7gh9AyccRPp31ObaLuuldCyBvn7oT18QiIvUu3upTKFILC0ZGdfHG7Lvr3m4iKTjmiiBwF0NsvxL1RSgoXfGJ5m6NVu1Lrq0E%2FZouiat8MMoJ8oEn%2FIuOPWh94oUsj18YGUo7yUke0BxnWLZOnHOlWSUYXU%2FQmqs00xpPIi%2BjE%2FoCXVo%2FPy%2Fr4gqhyDZCtpJpEiZBB1WngM%2B3c6Qsw2XPDWk%2Fu4ge5SDDRI1WCUt%2F04icc3raKyyTrAx%2BVUuz16DnA%2BI4GtHkzNpqSG8%2FKWMf%2FwGEX9e%2FBsZ16bgbfYnqRmRJUR25Clcjk23nv%2F8QjbGpnnbOcj9gxAG0oIhJ4XhndXZCLJt8JX%2FcpZ5D5RV8dUw2GZMjCDvyN5iic24Oc81NqQPjVAchYN9kQCFB4ZxW2KjeX0OFZHyojRcK6lWzPMewsGGcXMttE0xGHDTZ4gRtS4kck8iRcin0U1sNjiC0A7PFHQB%2FL0zLdwAzlHjIUTPmi%2BjJr0gGROv0CwrepMwjLv%2FxwY6pgHTsdNQ17im8Ofu0mZDbxc%2FPPUdTXitmMKfjucEJ7XL49ojn%2FfgzYrgUbQBsetovII4PLoAavzQMpw3mloFPkn1FfkDJ1E3jcP7fKcNwsZm65tF6RlnhGGiQJV4a4yzoGEeicjqTlqnxOdQIwdfLRIX8Wg0Azyw1ePMXLj59ADWrDXmRjOTH2GLnbRYmpXhbBkqVML%2B5e66rK8KZAfAM02kRwyZ3W8K&X-Amz-Signature=0532abd047a0e01c90842bd71a215ab16580518681edb72b5403a30949e045d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

