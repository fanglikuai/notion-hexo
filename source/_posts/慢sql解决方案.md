---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IA6XS5B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T120053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDl9OVmrV8aUdN%2FKCQuqBfUA4M%2FpSqRTg9TN5VezADgcAIgHKJFiTP%2B%2FtctxoZmx30%2BWcQTHBzbvHwf3q%2FryzkOtdAq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDNWUmBeESny1evIMtyrcA8MKxxd5sff0XZObdzt3sbzh16dhTbYMWrgV7sRJul%2F0YxVS5SlnghWWOY43rkEfoG%2FODPKrSUemjlC91BQGGwJ3gzvZVTi6df2alboc%2BqslE8mjz3jCh7rd83VVYGdvxzA8OpwVXPFeqa%2B57YuFMMf4KWyDlY3OwR%2BdQrpzLi3yaIqwEGQAb%2BsiwcPIUn9F4MJcDhJojh1d4lV8mf9Op%2FLjarl7ci6Q1%2FNlmkkF%2FLG4Pk57dL0JpI0n%2B0bb34jzxt1awkP5z3462naxjIsO%2BkGuPHg25dgvkKJfz6ZZz%2FOG5zK8h%2FlKXGhazSN3NPmtmMKOq%2FRzwTzBpbaqXLsoA8QVo0NeQl0PG%2BlwQoVO3JHQBwyy8aZk32koKzNI%2FphCjEA0U9aYqVasgB2n5X0mjuCHDOuXqoxNJRte5W4fr3aDpqov5XdO5uGV7m53GQwHL8QK6spxLXNxYHAfppX2MrtHGy49haB%2Flq908WvZfI08TwU8NrqvH7xPTXsz3g87LRYKtCiVIIylrP9mtKxsMzgdXvwgjOK2m2OT2EkyW7UkA0G5TmXF7u7JF9ihirm0%2Be4HzlI%2BvfvySJTQEGCAlN%2Fi8ieYBE70zL0Q7srdPFqwWdOXHt%2FkNkE6olp3MMfzuMcGOqUBVFMxPICSyyMzTdrWiHFTIV3zWhDIsWBZbe1qjn%2B5FTwivhFtWmE%2BlAtq6QpBri1nGftousA%2B9gsJSic49l7gGcdBpNs%2BaI7zPGlhLNzjGTNigMInQcIydfkzjQ2wZwCFHE1QoonSIWo4NouuIhEXnszP%2Bx4iPJ7FHF3Op9fuHuoGFuCbPZrks06wG7XH5lGrgpojmXD4EyAcKbxIaEZQjzr8zq%2F5&X-Amz-Signature=df363299d83dfb50a2e66cf6716eb31f4aca551cd2099d7d06298af914c776a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

