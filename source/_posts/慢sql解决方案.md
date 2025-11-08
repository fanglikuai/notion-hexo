---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZEGTLEO5%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQC%2F7G1YUS6lrhEGivs0KHxAwIN9n4tzuOqxQ44Iec%2FnVQIgeV1s9GU9UV%2FDtclH0z5wbHFL28mqKBMc0uqwaOuEbgoqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOcnihv2fjTM3m13AircAzqlGfMHvlmWWD88nYVhiOQz8e%2FluewrxkVidyaJGkS4HpUQOm7Qspml3s%2FOqyTIR%2BuQjfT75NVs951GGGFDSTXa9ZbeNrH8vO7m5Fh3iMtkbvP%2B31AVVIWnKOyhwtuOWyUwXN5g04mYyPCoETc9o%2BoYVG2sDmDqlXxcjptQPBRcGOySx%2FlZDGbW0aXh0Qe0Vq2XouZwqmNQcAumncslXii5AAkJ5a9oibi329vpVB7eZ2cfcJ7iLIZZFMiqtxiOvtQ5zT4WqyOwtCHgNpNpaHOCXKfKpc25xpHzyHgfiizsL9bkbtcq6Y%2Bc0Fvy%2BYztfEEv9lUQErUfa1ZRLiuSkDQPi%2BXW7VqcgcmGDv2nQ%2BbD98eeos7U5qjCyiZWQ%2FBwwxHS%2BVnVY3vFjqCSr0F%2FMedGm5mdRsz7%2B5AauZvGqYshDT4jU%2FrYe5cUs5UxO4dPVNLrcEs694UNbYlENoB%2F7KZ0nVmr0XkxBAhGN8LlnQ1bahFev1J%2F%2FH8uuIqWyoB6OwY66%2Fz6ZsZUBRrA92orREuTvrnio%2F8WboO15ahbTmQlm8NvgSjaE8WXZPzZmg04A8HOAM8LNvmwhShoiDCCkZcFJ2dOLku%2Bc0vHzbXfiTNBsE%2Fa3eohcjDsnlZ6MNXFvsgGOqUBcLEZMspVsT9rVnoublgbmCjZhvnOtoZ4XcR3ZlMw2BVniKEUm2%2BkO%2BUouZR3KGmRzmO749Q4Ff1Lt0SLS0L7lrVsIQjs6BVLq7iySLRudBCzH%2BLijB3j2E7GiR1QtjHDDKjwNPWUJhL2yLvk0X7rd4Sr5W7dEmPJ%2Bqxq2iMBeO%2BXCS%2F21dzYJ3a8CRyhUcp2o17pdf22th9hCv4k4gsDOO6zvz8V&X-Amz-Signature=2fa3bae844f05d0b13c47045c153cdd254405525c53a79eab25361137c393b44&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

