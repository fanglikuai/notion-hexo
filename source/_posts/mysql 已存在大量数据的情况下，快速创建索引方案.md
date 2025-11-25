---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UOKHPHCB%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T150041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAVOJYToJ80AwPuNzgqMp0cAicRvSSUC%2Fb1WXAr3RdU%2FAiAr7GjXbe0RR%2B2qZrbcyc3CuELPYNUF9PEi2tlCAjaBrCr%2FAwhwEAAaDDYzNzQyMzE4MzgwNSIMUG%2FPep%2Bf3aJFw2NAKtwD9rRCcOCmRbxbPXGIhu8Lry8f1QG%2B39LU68YdFCL5NVcXoxCG7HTOCKqj%2Fb6PqQ4MLdmXQuKSoJ8SjECs7mU%2FOHadZA0BeRJkx4stOeW87rO1HsQMGSW1tnXTmYY2lIzBRGBG%2B1zX8jvD1UDVOFFvM0K3ig30jFyjvoBkC8PWZnSACM8mCLjvHsZd7ZqUHjR0kBiQfpELiMfxUNFcHcKa1wochIlZehdzNPPR7%2FQWP1T7Xa4ZVD1wwVv2ue%2Bt0ktxcAeoTJ%2BhrwnQ6qTE39ZV20kzDLB7f16JhYF2LDsjpLKbffut4PuH%2Fk2bLl9pf5KMcgXnVfR%2FPIIli2jCqABY9qYcBTy%2Fky54M7AC6wDxI%2BUYT0KpIA42kQnOyWPN6XosNfPeFrPXasaRTGHQzwywTRF8JoFHzecZ4oUxctgMtcAfhcUR9LuEI2bUpIwhNqGdR7uScriVqtXzKgATNMUXZpoYazAZIdq%2B72Ch8AaQMPKJs3yqzcSZ5HbBnwobk98MSKX1hOBXRyAQuxArQTeJX8nfMu%2FnFOYRlAwbMG4IIJNomAJP0p5xwRWDGfywZs7KZnxK%2BfVDiCckyGFkaPFwSOzfvnb%2BhLPofNIRowHpBxzGqwJnzmqggMwPvLEwqYGXyQY6pgEiBigg2oMXik9KkEKaG3WLrzmyJz1q3JscqEO5WoVRnTu9PBeuWKv37Z%2FKJzW7iELcyetOtdhCOvV56MhFDA5gueksn9MikV%2BfjcOLrv7VM3r7BD%2BWCXWZPPytRS2D96k3V6QCuTvCR%2F6NwuOoMTnFRt9jv0jhP5Rm2peuhsbEs%2Ba2ASIxLqA3Dm61QawfhRaClYDq9Qt3ERbms%2BEU7VC12KKUz%2Fy8&X-Amz-Signature=98762267380f78ca1097b81044c8572d974f7ed79bd2af36629d61db877fbe62&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

