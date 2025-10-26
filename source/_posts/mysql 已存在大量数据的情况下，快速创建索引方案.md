---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZJY7ADQG%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAExFR96dtswfURqJJEjGmWs69GK5JZzeRAZPI94z%2FryAiEAgsrklYaEfqTUnc92HdRxfbuap07Uv8wzUgkbP3KmblgqiAQIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOvVXUR%2FuIrNg7CAwSrcA1wleMv6YcK8hGHZUqy6a26NsiZGI7Phs3aGFDtPmdeJMN8bFp1i4LQymUF1eK7NzD8jiRffWbR99vwStsQktCPd%2F9fZmQSHMkRRXGEdLKHKsoqGIH4tJe240TT%2FwxlgnP%2BM8Jzb5x8rhTXENZLOnCnsv15a2do73wzMqLls9TnhQ8rMCpmG%2BQAA%2FV1J4e3jun0mEF9ro7LIZelDhkvHDGAeJvx8%2Bmrhf2dD8XkEtR2ciUiRr6n%2BgQpofrG%2FAQNupAzK8EwECdWOfu0%2BZWskSONcmi1%2Ft9eAKquO4rgrLIgjeGzyZEoL4gsJzhw6SDJMi0Lty2v5%2BifyaeaQqbsUw293ezTx%2BHlgV1xSrH5Sqpyq2VbA65DnThw%2FnPpimtC5MIkFUc76NOTg352gcb2NQttHpz5nmSWkex%2B%2BICHOOq0GkgYJGKYvxLDpUowUgKOFta%2FJxqL28nyJZluzPDRaWe3RWc2t%2FvIs79PQbbQmiEEBGImGn5kwV8%2Bc6TyP7TVEufEVImHE1tOMnpZg7nsTKQYXgcoVOsWh%2FULMoS233674YZTA0iBzHM31w83woXwCs1qej5RAp8Bb9xH1310dvbuUrGC0yZotGCRxMWiTT8EPCnDaY3scN2EmE69UMNDX%2BMcGOqUBOdVjnGL%2Frm0JA5o%2BbRKQ6wyy9vC8Usc4Vh%2FZXiQG4C9xPq7nbjbCTBBpmJZsLxmLenH8d6w2sUFLle%2BI%2FuJdmrhTknDYa7exwSfE8rBvaDY6cbggepZe1K0PqpdB3nrJnyt8R%2BfD0cCqLtLA4eveHHn8007p3YW1pSQS3NxoBaQYrd%2B1nHrTDKkACpKF3P7nMO%2FbOLj6eAw3Y5vZztqYEO3Bmwt0&X-Amz-Signature=b531da04276a8fb890d44d04f76128126ffd635248cde33982f7430ab7601e80&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

