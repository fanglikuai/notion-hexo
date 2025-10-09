---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EYMOJJW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIBABmpghBO8x2abjNlrfL5XauG7w6TENFmedTbQ6ZemrAiAPwAO3eh99qe%2BtcHKPZ7M7qNN%2FfXPReq%2FxKQ7M9uLQ8SqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWuwfMrzqFGg8SgaNKtwDAAm68rJnU1uUjHm1vBF6KOJ8nmudcq9gbWyb3VakjjqhHj5jnfPqeYz1PNOkhQr7MDBJvtpH1JGgoZiOXp2IlF%2BLmOm7WOKG1u7iB1%2FyREK3wu9SVZ6imw2QOMRk%2Bxh6ehUrbvD1C7JjVl6BrJhOHBMQnRQMVAJmfl%2BqfBTJkz1qb3iWARRV8SIzVH5GdHZerCr47PH5e0ao2OnoLk2SLoPmidATCqeywOZd79fIjALgXpUFmSN7lBYULQj20wXsmfa%2BDVWvcBWypBNgDCJ12elbUD839wQeoC3iDBSmvvDp57XzZvK0M7M%2F5tTnG%2FUx81C0Q7xJHW76pfjO67aVqBGxBf6i9XTPy%2Bme3UVohFYMaamFkR%2BIbOv%2Fb7QELVRgfhAZUDMegT77YlWJHIr%2FQFRnKN%2BZUGQGi1rYGE%2Fns0a10Z6fDX%2Fo22O2mdmWyCJvt7afc8jGDcIfYldhCWITfCdpw0xEUHQRrAyBxehb91TxMOZIyuIOZiwqXNPMN%2BhZwikp10rKEDxrCi4OiZiDRMcHGLULRToa%2B6Ju%2FGh%2F%2FKXnaKCpL0bJlg9WfmLNxA3hwPne%2FEjKXGncNZMUbKjdp9etntsUMUm6JmUhhB2Qz%2FtSzyoNe5fxdsbKLUQwp6icxwY6pgHCK0EIihSj2pbd3umq%2Bt%2BcyiFBdiY17urQPIrByXavZqIRemcX5%2BvvrTPoebsPqvwC3v%2Bw7kateaEO5OSWa%2BWKzx%2BT5JRK8gGv90T0v3mEZ5Xu5aSYEU1GGm1tYBaUeFZLOtnZDVC7gK8zO%2FV2CiueOPfjLpOpv%2BO%2BArSUvg6UTInVz01109X2pWy8wDHjSG0NNNvmktrC4Q4IUGUBPEPepfbTgx1Y&X-Amz-Signature=788c5ff7b0e0ea9b926b197cdb57cc2e2b5ecab0e880c8ee57ba78afa6f809dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

