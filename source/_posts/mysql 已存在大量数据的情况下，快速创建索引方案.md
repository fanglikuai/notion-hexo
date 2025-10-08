---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQNUUFI7%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQCj6r1FSU3zgamnby%2F2wfzt6poLRw9dpG%2FA%2BeLQEneB7wIhAIqOWbURd0FsL6cwV3HOh2OaQQNDUwdTFYbIDkv1marnKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3biIusUuscq0yt74q3AMW%2F%2F25RuVZyKGaoilDtCmpoz5ODe%2FdhVprrhyid71h4PkOddDykHQaaPrEQCDxoS7Z9ib0En5g1DuBscFzr7%2BJu7pflDvHA5Z9yK5m6kg7t3%2FdAXNK12xEo0abZbCOs0eyJJJWZsCgEGdg2fP%2B%2B0ALqcbyqTvcLQePDH%2BWaBJrCXrwZXIPznYSmZvItI8Xde%2BYOA6rfxi9WZMcX0hRIyCErIdoqmdFol%2FBdJN91ZsaQrPxauhQlRi%2BfzQBsQa1OEPxZ%2BRJG5sgp%2BPpf%2FFloG%2BodqsJQ8pQ5lN8pKLiMD79T9oXLIfqRTmOBn4M1%2FN3EQLa1Xe%2FUAwkkfMOx4gftaQxBmIzKR2tjxAiU7M%2FwRgfrTA3QZ41BoIgutMcUyih662MKiprOGqd6rIY17BZEq1SIwfQ%2FYZj6eDPHhFeknCOUqlhl8vLdtbfJqEUcHn1zZU9JLnIKim1xykfCtVoJb6lkvB61kZKVHpdBLjY0%2BQjccxfVVIc%2Fbp4pLjEJrs2EDcQM9ySjpZa08Cq4dima%2FRYn0GqQogUE3IqRE9Y1zdbVFvmpB1cTRXzVgKbjc428dQzFO%2Fx1gHPf7TYI3VDZ4DbbwQXyGkYSCKA4sQIhQjepq%2Fn9CUS7xf9yp3bXTD%2F35rHBjqkASOUBLFw267j3Gpd8G7VGYxzxi0R3PuB%2BtNtsj9GVk2ag55AyMUKZmripQdcR3WRysZvNjPS58%2FAaG3B76x51pKEvkLTqCEdRnF4g1f1%2FCsTTgfBNRMfe%2FakmuyJr11iEQRpWhysyAK5avNx18BQMLDxSVyAvTSlmWOIB7lydAutHOM6HqDNikrEUZOvzMSxRqrF%2BO6DVr3W3un7avNj%2BmCpLvzL&X-Amz-Signature=8e8edbd6b3f0a0e237045de85ea95bd98e7950e577baf2133b3c8932823ec221&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

