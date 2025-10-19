---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SIM76YP%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQCLrOlUH%2BPr3Kx9YhcVA2FzNnSEZHtoDDZY%2BM1OnbohMwIgV3CnSA3Van9lGipnHLHIaeg8dDt5Ddbbe6MUYMgaTTsqiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBHpzl2wKesvyycHVSrcA7fmb1XNcWIKANbluk%2B%2F7dZKfxsmqnn85XdAGv8IOsjurm4TFSf7qYoUqJPSnHYOEnKtgE%2B24GFzfGibYRW686JQ5vYslzf9ZBzH%2Fjke8%2Fqdx1EwHMCMYZRA%2F5XscgxsfndFHSjo%2FS1VQlGplgie2mMhkwj0aXZP0Yl9FBeV%2FrpcGZA0oRBfBlmWNNhr4AWaKDtdjCZwMUl1IYn03jZYJbWXQpxq0jqY%2BVfwMbwoIlbGXYrOkMr8r4sDZuHQ8eF9M0O0pb8p2LuTXzpGJ3NilooRP5GvJ%2F8nYWluv4r8B9MUzdsqgiHdugDGeh2JGhIWu3UbcuME6xqjoxzAy8bAc7n%2Fi7pkks7Ugn4eRQxe1lznb%2FwLUaGUnNXrC57%2BIhZSHWT3rHJ8HN8%2FuGrIvk2LFWDCs8cT9%2BkTk%2Ba%2BBykdB9%2B10G4tZ6zHj02t%2FSi5XKcRHz1Vd04z0mC1%2FTI9CIHkapHS%2B954IvMeLV%2BUJ7wUDGmCYzs5q7hXVUWZQAFrtYSs1GhD3TiiG964Ef9Igx02xttAKtAEZ6cBoGDMrobWkMWjUP7CXYreuFmuufw0cJnLkbXdort9GVnKTncpoCPrdwjv3428qHIuPyDswxe4qXu0j5YEgZRCOUAHQ0DQMM6X0scGOqUBhafGMwLVF1xvKwMsITa4E9qmCEpiG%2By8vZkoE8coBzSot8i%2F7ktkAiBUuLgTkXRxuTQqpx2455iEWOhFHJJB8p1tR5%2FocGcid669Pdj6TPV7wezV%2Fvfa%2FRQeD1zh8UymhLUGZPHaESA65EEvfeWRKQume%2F7mF6l9OtV2rTDVYXXieNSTnLnityxJB45CQIy1ZgYvhxoUsxNYqbHOk0KYPWFj2h%2Fb&X-Amz-Signature=1f755542902e9806d9cec1808894f2f084b3481c5c7a0ecf937a712e8c1e31f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

