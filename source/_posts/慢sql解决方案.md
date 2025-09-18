---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VKUMN3XZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T120041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIESVOizWbIA0%2Bf0idUb8rKJSfL%2FMevK7PNZXRKMZqfVoAiEApO3VZgNdNV1e9%2BUlftj8z7qmefPw6m5Z9KUZgv9CE%2FIqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH87K1nPQt3%2F%2BfcS6ircA%2BuuRtUSzAMb5mro%2B0K3zCu2r8zmflzYPSvHHEv3jIGljJVSbMTdBJn71CQSigLWt9ucKLFoprnfGmgc0j9hx0V%2BvE3HmoShMs%2BHgs5YSV76gmAB1FnQ4G%2Bni1apt3WlYrWQwLcPrITPKk7dMkU2BMhth3dW2nsKQksJTDt3KiAtj0xeUG6Rpd9dTcvlryeVuonSeq%2BIvQ7OAtaDss0gPg3XG50DXWVOi2UXmMQ3H9MZCrh7i0h0IW8fpzzu7%2FfBwH%2FfGQMQqTaM%2FidshezQpDyYUSkW2cCzVm4idmsSwXpktvSoaQWfSCF%2B6EWKbuqjn%2BB0rQ933uwhjOtAL8QsccN4R3DnOMbMyFwFJ166BBeKVs8IL0NDgz2xgqtnBRARTNYV5W3AdyXPt4XpiK%2Fglz2%2FpkO%2BMJ5JS8iA45PTw0P2OeL1NHwS41mo%2BsbZ1Y6w%2FtXrR8ol%2BixxM%2FiKehoV%2FTS7TbU6Wqi4%2BXGX0mYCepU%2FeM0ZRQVFXoQstZfrtdo5KGjJYRDr1RfWsjnp0g86pEhsAXK3dWcko8LjDh2MnpN6CU66TOUVo2TjXvJKx%2FMitJWzMneEcu%2F4tNQQyyD%2BkZvprKv7zOuTj1C2TF4wdsZdB4ELOZjyyGTiefbEMJTZr8YGOqUB6SQEV2FB6n8B0E8TsVBth9uKV3ejnrfdU4DV0Sp5wZUfDYQRkXDIFomIDKQD6TM4NWM1Y4q%2B3mkYBENQR9OqfgCWouLxlcb4eUppkr2262FNlWtbjBa29d%2B7aU8O2xTGttmEI6Gn21dSEkuK8J0%2Fze49jajq%2FkjNMfku%2BK01KVgl6Jc10fGiBjohkmnfHFVzOQOYoAvUBDXXfnPPVIL5lWrJcss1&X-Amz-Signature=d0fff5281eb34fe3701e452fd3809c577f5d6b7c1f9d79730739cf355e440d40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

