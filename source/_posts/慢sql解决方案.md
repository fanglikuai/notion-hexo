---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466545WSYR4%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTT%2FpG0u5j8RtOXUAkxQxxk5raiLF5rsZGapnMrfgc%2FQIhAJYBc8YVH6p%2B5rcwxTs7%2FrAECZyAjlx8HEWUnaB%2B0C4XKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgziLzkXX95f8Gayal0q3APxcg7ohm7OhYm9PFGOudPo%2F4K5ucyFs45opBIFWGK2sR9a9d%2FYHdRFnhlhZYCSrSaMsyxtBL16GiF8RRXx41KZbdD4oopwToQbdqqF%2BeLnzo2r0VXY%2BdtND5PwLWsEBSuZBVDc9%2FNRKRRp1uDt5rXmGRe7t3bVzKHdXwc6JYcSw0W4F571kh7642%2BL1Z0jaF5YRXC49RUizihDJ%2BY6Wn5GmoiIwb9FlwRpLUxuBB57SqGG0t%2BRqvSMwi4Eu65I8%2Fa3Y%2Bmg0ntckmeWXwHRASlgNb9Dw0nID%2BMAH6ZOlVjc9%2FxpEUy8AuFvfJn7fpeLfH3%2F%2BCN90g4Bj7N7eWYH2GoMbZgXCHu6bs16FYw32fy8lT%2FycT8beTmna%2B%2FtF%2BLAmbztsQr7y7Tq3cW3ERltUhXE9TMpkL4G622NzDxzeR7MyOo4JLiWxxZVoD%2FxeEAYXE9TWkmUHMuV0eHfO0yfCSAiGPLSl2zfKKXFnn6WP0uulnWLJ2Q4%2Be%2FU%2BwlEMRNGZ6tkmDIelKKKfX4Vyv5EbxVbWqwe%2FHknmWMYMPnwo9KmzcRiHjeDqwqIZTlhB4Alngge%2Bb9njUcZI%2Fvi50XSYCiHrQLrtmtqxXIpA9JQBxMHVEKiOJJ%2BoQcH8Y3AjDDZjpvJBjqkASQpiyfK%2BaFd2gMfgGgIRWyCKAU77zkI0aR%2FCxcwmuLDssy6Jxba1aa%2Fll9KCvAahtNgepokkJHmz38CycL9ifxpzAJ8DtMIh732uLPGIOZx4d4T%2FcivAEbqvR6pajP%2BWA5L1s1rcBy8XusVCtwjfIOULHDMPkbnVyqnjl6TqqOnp0Abr%2FFuUh5oS0iZsYbMG3TI8R%2BU3VLAV15I%2FSrtDXbcQgex&X-Amz-Signature=0318e129572b3295874c988816fd439a3765414e1e94c5b3cd873f17445da7ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

