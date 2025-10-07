---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TVUXJLU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCZxPwr%2BCCGYkwvnm8XvcO8scCCdrCBosRvPbIU8Vc0qgIhAL2GpKsqYVZs9r3gMli5NnEhQaNvBneESFo0rFdtcDdkKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyz3CrZZxIEQzScU5Yq3AOmKyzsD%2FePBY%2FU1bBwfeLAF7EP4NSm2fcxxKWri1MaW9oXzavJu%2BfJS7nbZfz7A%2BneLW4828VDxCt%2FECL4mrG%2BInZ3QldE1I0CxkWnYutADJnEria9y4lGIdRUVm9o3cVvZTyutFCzyBWz0MrByWP%2F%2B0YISSVAahVP%2BZR6pXBTIbTei0Tc0LTKkktcL%2F8JtyA3y0DwX46jytyOmtwSAB8P0iMm7CTk8tmQSnzdfMkPYKoDU4XfyIvQhjQZX%2FYOBj5dHH9cif0C5lftWxJsFB0OMjKx%2BtLLD6L8bioMFrGsMUjvrzbKNijEsNxR0HU%2FZLW9iaHZOALoCuKwOQkXZTRJOFp61wxY0zbsDkEffn5uDmXr%2BaeNGFNsqN7U1U64bHVa6zbZ%2BxgaJJe8%2Ff9un19yhWOWuYJIEr%2BDI3FK%2F%2FjldbXit2i27K1947aU%2BbkIPoSeBNW%2B8d%2B%2BHiu5fTmorldf80R2REBjCxO8%2FdJIxGYArw1Ta6md3%2FCNXxoU8SHJQ%2FUFXeYmPkptqyT%2BV0ua2YUYe%2F0%2FoL0Qm2XM%2FnGrCcznouNRNg2QfN1bn5ENLfcukaWr7XedexH4MSCycgF7Y%2BDt7dhotIiDABeMY04EQPlpCpsbdLa8Fhqg8UMAmjCjk5LHBjqkAbfTTm7CxizWSDzGvslo1NUyICHqoz7naNklJG2paOLBLhMiwApZjlbgegFvIcJ%2FVWbHFJX0cv4E443kOF7Jdgxt9wTTedS5RPei4nGcJZcwRClQ8PWh7ZpVvh9IetzW%2BUkq7IT%2FTVNjOM6FI%2F9%2B2r5gQm%2BwZzQXhbwO9od11HtbcHbcNemDYsz1mM9Kd9pTxRHJVWKxeH%2BZRdWmKhTudrQPeqLn&X-Amz-Signature=72221e44b7c315081d476da901cc361b60a6c9cdf6fc5e04e1a059d00f3debe7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

