---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2CC2HJE%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGCUk4mxHpp77kZBVoT0fgjwQdtRxffIz9fBhMKJQFeAiEAt4idbxng%2B1hl8yMUmvM64t8CZ2u%2FRWZENRouoyi2H6Uq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDH%2FANPpwinA8N2XPPyrcA2%2F4H0Hz59XCSjh97atvXNFsHE0ndEaQIDHVK2jqlEV%2BDmE4uPxD1fhCM7Ong0s%2FRIXrEgDpXuvJMcaWJi7F5qa7u%2BMKARUZf%2BoV8VqUU2CG8SErOH0Kiqzdvc4csRdsU7l%2Fec06KfmRLK4kNcEBvDjW1hPb%2FSBboJdDYbFYft26iQvx7%2BFMc60scMKiS3mLDwS1hcDBodgR7MKwahiLclwO%2FYdZG5W8earj6luCBdWoh1%2BqBKepSzQcgDtlUysKR0%2Blz4Ir7MqdZtbLKOTFnUTWw2lg8KU9bsjewbLKk2AQnqMqjrQ1bstwO6ZBi9EoJNg7o%2BoFj7AKAp0C7BHyIJlqxB9ELM339dRIW8xinbCWYNIMur53rk9%2BMHttOjKwKSXWtp1Vf0PoJmIw%2F879p1ftjPRgiTA5uBNABG80SlV%2FPS2vaNO2Lg0UbQRJB%2BPwZh3r6EeIBt7uh5G2Wz4Ou%2FAJCL1GfrEzxPcxppfyIa02qvrbo8G3dj%2BKNonCRSI94khahbdk2QX3H%2FzzW0nINEMplERSTj38HPkKrlIkjWMEvXH4hDgkuH9tHJ4eYG9xoPONe0fp%2BZ81o%2FDLZQBz2blFGzs0ifHpGLEVm5I47EO0SFydWMN4GdWDkOBAMOHPgccGOqUBBn%2FU1jJgYkdcWuzQethfm4ZrfI6jGHki4jdPZ6nD74Jw%2FHnfzLYCb%2FsfmfvO6lFF1IMJzTOscKEWVxl09gwxMiO0lGs9IGlSWW3AYae%2FCOYQa01sskc7ePEuZFc2lulMSO5CW2Ah3mysTDPZf18yoocfSsM9Yd2sDTK3iDuLRSs4EZvRHRWjCyf08GrKlw9jIwSB%2BvrY4t0l5tEor6GgVaQ7KcOC&X-Amz-Signature=82ca724f4f337fee7c0ee656341dda643e8f38d1b6f506bbff43eb1d86bff5a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

