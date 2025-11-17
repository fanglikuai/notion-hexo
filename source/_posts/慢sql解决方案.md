---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFM3UT7W%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD1%2FvVs0C7yYyipem1DLej9dbuBvzT9cCHmDdvbpmMoywIhAKFQWtNEdMrH%2BpboFM99MO%2FxmaYXP0VBHtmZjnUiFJ5TKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwmMCxTK6d%2B8Cwbqgsq3AN1MV5xJxdx2JEi6VEjzSQ7Ng1usA0tHHUPQ%2B6RkSHWtgxwrmrXwpvMEbFLfgLbLV83BghyjJ2xgGstXH1PCpNN53cw6Nom9AqIqZHfZAoolorMbsUON4PtgTnf7duz5ZsedY%2FCkU10ZNbD4Q5%2Bp0kxJMUDRUL%2BLbTr9ViMCTo4PI1nYIcCdam7o7yhSI6SapgrJxZjiBHPBGPydsj9QLoq9QrJQMdzPx0e89nu%2B67m7DM4PyQXiv3FnkwpnhVTEgKDhkARD5S5K6mKmuTIkLKivt3gx8Z%2FTO29dXmagsVdXUoP2XhjDtNkysZHLXYv5E7p1sd9kavDFPNj2z%2BqBFyHpCkNq7IYAqqH43osYixOI0OlsamM1bzc%2BABpERAWdHNgcdKrGJ934KjjM6h3QHZ8mRarzBHU4HelVUAev8K8dnRWtABCsKIW39D9IkxbjZp8eSRyWc7TNM1a%2FNNR7iZHywgPxpyE5Mz2ZY%2F1GlIFE9gsZvVVj%2FjUZV7%2F3u5ybfnusoe7OQsdQqZswEH6u42O2pR0iv1twrO44Z1ELkVkVzZVN7RuZ5Y64gPMCKY%2Fu0KuxLs%2BPUuCRpvhN%2F%2BnPCVLuZZnKBs5sYHgYlsG8LsqykJQTVn%2BPVyLrk%2BuszCQ7unIBjqkAepid3dT60krYzjDeBqJvQO77Wi%2BVrbRcabUs6BPlZLODV5RqaPVvqVSDfoaM%2FWm29U%2FRzkAhXJAzdH7ctzpXL3Uu0NOySjhi%2F0L34triX5w0k0mMqn86TaIrcEUwkZoG2OqWAn9s9aFIBdtFVQ1mlpV9WZOqpIwEwZXc9k7fBINl7Vl0SmOWTSE7i%2Fb2FG7aL5iWthsKpVYw4PK5mqn6XBHmlRo&X-Amz-Signature=c5d7f33b28b27d23296928d6865584c535f43c550a3a5647be461e933a09238f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

