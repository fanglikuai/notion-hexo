---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS367O7I%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICoEEGppcG6plVGGgS5STKfZpmLhC7v1RT38nhDjpXQWAiBMShf6tGbZyza3QLfwcMIkoJdb44NJKsi90GOwH4MCoCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMrnaceImw19HH7IfKtwDnwC0tv1KStQ3EQgi66%2BZvQDRHetNYjoCBySq0w4K8JCkEbyzBMcJ2x%2Fhr%2BpmMICAgI2o0l9qIbIq4Zo%2BOhCPL1c4yJBTR9sF1%2BECMTmt%2FsTpAG4tLgDDzrZkjX40b2RLi62hWL6pL%2FQNnkboR5b1F4mubTtKeAS7jGUScxwjBKZdrD1oyQGiWxKF%2BQDzHZn3g%2Bpy%2FX%2BWuLf8OqnlabC1Ceq5HYhg%2Fj47ZQGL5VAWiqcvecfiZEDfJnTkYsg3nU5KwES0YS%2BlhNGkq%2FoHywZV%2FbUgptV%2BgjLlVvIb0JTx8D%2FKdgofSs6bN5SIKBXPxS72BIa4qZQZO2hnMMINRyCsyvyKNirgZhdnzjy%2BnlDQ%2FEK49zswC3XPmSYCi4ygwBuGsMWDnfpuC9awi8PgEqwxW%2FVIwErNENm9t14enNP5qmSVBeMXFmlhaqCGFqecuLQQ3SsIEdFiSZNWTNJkYo%2BDhdOQru%2FwzpUsngq3QeQqNd5mvKSUDIIVpyK51tlV%2Fl%2FZEE70pIK2M4J8B%2FxHkO45JVZh9bXKmJPt0aXIhdziQcdwoPvFp9a%2F4KFaN%2FIORG37lC%2BRg2SEwWUeyVu7gwxLLRA%2BdZjUFk4SSxbb7%2FFzhS8AmN7vd%2BQO23spEOYwi9f4xwY6pgG%2Bf8MbrKj1D%2FVSpj0f%2BQpyiI7XQU1VI5aATzaX1iWwsm9rsSgNGDLoURNLN4KyRHuwgE4uXX8Hg2qT5ipfB%2BC2J0WZ3LJe2UlOPaEtqTZrbTOpXYHh6%2ByuBLleu2nyvtA0pEy10RUEks8ZaiujuozOB5OkOD91sFx71rltLZUZm9darL2MZXv7r1ZTFdHoITDyDkw5FfJQimTgH6iy%2FpabY2GKKpp5&X-Amz-Signature=50c8dade7a8d847f8fb0df7ba6ca177513a79f8bcc06f1d4a73b0e44e90aaf6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

