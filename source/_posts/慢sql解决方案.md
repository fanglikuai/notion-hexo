---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X3UIDNC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCnxl%2Bs0rLR4TCtSDd%2FXaTlDpVMF11%2Fq0RjrrkWoGW%2B5wIgf2uK05r%2Bffwt%2FlJelbJoPzB%2BaOfZ3Mt8SZu%2FOkhSAPUq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDKczavwspzpoMJYVfCrcAwAAEt0JlBLX1%2FNxoaZm9kxqgTc1uA7HlWvyCBJQB8e%2Fzuaqvm1oBDKOl5K8iR70GUnKTePY5rhItPm0CrMQ%2Bc7%2BNKYBDC6t6Em9%2FCjkC6fw%2BB0wwwZd6A%2FqjIH3RgUMyViApQJbzUN2aXlGZIygkEExWnl9%2Fud%2BkUHSxiU5Nd7D8h%2Fowv175bZ3a5zHRyhjf1nHUkva8AY8yFQDPMipzKSnGrjNDKw%2BD9XlLWPg4BeB3vavUx1PUbBaGf%2B2%2FVCM8z4FBnba%2B9TklxOOfBxfC%2BoTtbrBzm8E888ZNsRBpYh%2BD%2F9gopVlTNAXlPbJRIu8oiH01%2Bvci8iUtt65bxT3QByBVDY9Q%2B0EaORwNCRkX54ohuIjohO8y6miYkzIIuzmaDACiaGbaltNWR6MLWxXix7hkNmmerfrq8gfrQUCPGLJUjI35FNjCVjHHkXANUp43b9jTBKr8j5XwrvwO%2FeVTRILGlNzNmaVFOZ1YO5vZircD8%2FOO4ClaAkcYY6zJin4DHBtK9B596XLM%2FwdaAGNcbQsQelYmLTdyiY0QCGrY0oFc79P5%2Bx07NelJXzT8KWFejREbfaxdsDWJJBtOSdMkPr1LjL88I%2Ff7coF7HZAktbbA7hiALvoHCExWc%2BdMIujkMkGOqUBaC4eFYK%2Fat1b3sCDefNK%2Bu7wB50f8fffK2kNFGiPqDOuQJSBUECVIycB6oNfQ1zutlVE9bo7eZvA3MYY0rEDsZwxAndGujqjEB12sRr7XMCZuRHX3UUmy2EzP0yE5UqRZfi5lNBPp6jbZKrumP2%2BUv7XDchzdn%2BhJbMktCDUQWKkf6avIRWxKAhhvAAsIXMFXI4OCuO0aH9KGKnuwbDu7%2BPe9YYC&X-Amz-Signature=7d957f6354a6bb428017e8abce5fd860ba68f12e229f967e0bd1598b188bc5a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

