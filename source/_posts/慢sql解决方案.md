---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYSHATHO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsrH%2Fr2ENmOZmlCA6GcGYjVzWmzwT61eYVt74N9lQz4AiEAyAuE1IkqAHXqNu%2BAmr2JLApotXR%2FmnmTyqBKqPo9nfIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAj98edp2kPpq1kBcSrcA0bV%2FwKX7Vki8qB2OkGs9Cj1PQGyaDhTTesMPOsy9egpSNGAMSOZGnCfMWG7d5YkLC%2FCCKlZ%2FWs1JZJNdBhfqRvS83CC%2Bw0%2BsLtJbMgrmLX33dMblXvaeBfAia47OsSknW2Ggk876zkG9nmZU7HOMy2Hy2BR%2B%2FNYD8PhuAj8qcxUklel3DYmtRKc%2BVlrGp%2BMkGxyN4LP2t6id9zPK4TZKa7xBMRlYrOkrL2IlhXky9evopi7ko6BItw1XGpSnqqJk%2F8ItazVyFwn2RHtd7GAEF22hBTSXxOOVH76wAc9Sjhop2lKYXwkKTyWBALOHvxNoSPJ49NakYm1ZnRgr4cDVLyok7t3Dk0uzcsP3PisrNx5swF%2BhDGocwXrE6MaShjZqGwPDCRI02r77WfZ5r51EmegxXm7eLOjlhbCw81BxrMtYRRE7r0Q%2BBLmLzTX2V0goChB5RE4g4P1nj1osOezVd%2FKlxbUxvJcvx2tQEfXK3zOWl%2F%2FU8h3kb8EjBXRtZ8syKhShRi9chccyCO9O23Im4YJjgHVrt%2Fnc%2BUfCZ6Nzr4BvM1FCmyOyAjMxMApuiS73UlyXM5CzwBLC5rRQyTA3ttVp3IrwgtAZGMEoza91c5NYajsMLVkgKh32D8PMIn%2B5cgGOqUBWNkA72ODGPgzgRLsd8Kn7bBTZmDwcfrBwx50neeyql4KltiErZyCW0021ZrHCnRrcpivZB7r%2Fc3lo88vTquDavkzxGjVrq1FM56IHUNVlK%2BINO41D%2Fq1VB4gnmi4yBn3rqUYXP6Noq6MvlVwcjvgpR3GM5PRmUe7mOVWbzB95J0Vf3I22%2ByP%2FTL%2FNq3cvkwJjsqraC3VUf5Y4HAkQwMDVOZXWhqD&X-Amz-Signature=6f8bedaa3e558b3590f3b132198165f1adb388d4bb040b5fb47102421e91a187&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

