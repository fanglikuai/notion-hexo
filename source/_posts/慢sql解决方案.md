---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH3CWWMW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T070114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCgP45uYdjnAiadIk%2FPmlswAgXnFoZNtRiWZSVYvzmGGAIhAP2bhXOE7HJwM67VqhJdlO4zKgd3baJ952yJc%2FRaOpw6Kv8DCEAQABoMNjM3NDIzMTgzODA1IgyiWD5bsNWL6C4Yl8cq3AMt%2BDDY%2FtbbqnsY0rM4in81PKAjfuM9DWFCCVYrIr%2FX9rig%2BlGI7jqfSTSwb0MO%2FmDL%2Bx%2FPr3AKoaoL9WVT5vO9tDqIbcwBSWDtwjwmEgHFByYq2y541lk06xHqZsXcXErKVM0uVMcPYB%2FOexdmbgoy1SvFxKbP%2BdXB%2B3Jlw%2Ftpo31arSoEQIwXM82s0F7gwScHHWRrdzWsieZY7iVRxZVOxIeXBTBw%2F4jYg1iN8vdliZO3CeR7OkpLcj1IuPTSIOYLr1SvElBwiUj5ScAVyKlM4iBg4uQaMCKv8muifVx44nZyoEOIdClMfQ0GSIcVudd1jZGGIv2D7aJ%2FEjtMUkEMd3hOZxcseQl0Qors1%2FwzttlIj04%2F23I%2FYjfksTmVo%2B03171KLwsSUW4KF7whxqfkJ8njabxdYgNULLZwiDT0Kb1%2Fo42Fz7Tna1y2d%2FWzewNdyVv%2BtCzEBkl7891HEiZWYgOYszF6CGqMONMN7pL2zB9GNtcemNagoLvBwHzdF4r%2FImfZT1BvKQxuL00cVhtpJOi5hUY%2FvkQWjrhmqCkzW4%2Fp66j8tk3yu%2Bn1J%2ByYd0rW4nGoiCvqOECKclWPoTF6EAhqlMUy97Q452i4AW8d2%2BQNVnU2%2BS5sk%2BWB0DDivbLHBjqkAZm8ldhhUkXceG0bLUrTvdm1fL6aMK5vvuR9rFj63n1hv6GcBYK4Ow3WtRkjuj8pI10vgEWD%2Fslcdp0SPvyatOKIDpZ1sQ1lq3dDqOUiZf0YfW9r1MAkYQ5iA76P0g0MZYf0U6LBHBg0FkKEEEpfl%2F9wkPFOOfcHe1AjrW%2F%2F0fumokBUMR%2BFh2sCV5EwLOc%2FsIcUqZregMjBLs5XrCE7NeCMjZoO&X-Amz-Signature=a85101daa9d6cdb638de79c6dc83a1beedd22ebf58f7b6c3806d2f68d7999415&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

