---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVUGGHHR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDVFfJxfuU5UpD8OuGoDlT7rhlgXRpdY0j%2FAaxkFRq%2B%2FAIhAJtAj3p87NTKXbjA4HV3CAH1g72WVICq00h4TY%2F1AlytKv8DCDUQABoMNjM3NDIzMTgzODA1IgyNWCP4oh%2BrNgozHhcq3APt0I0NePVaJn1Nnij2X%2FttqbhrmQK9ltepd0kPc8u5Gh8LgwAKCPoq31%2BqjVGLgiOasIvCUCeEMcmdQ077P453D2DtczR9S6KjhpnBO0dbJKxKi9XGvyBbIPgGXkmQlLirhvW8di6lCZTDo4QGGrVOxZOAu5YQ0gVRrGG3IrlEEhYvOUY5TZSKbFEhoXGjYHuY7kTM3r%2FquGc0Wh7%2BxPUC3fytCcFjV1%2F0%2BosLBADaTCoiqyXiLjkPin75qGu9HNmlS8TKZvi0dMIP3AGSZBjBWgf93cxyC25lgyaOFWfbupXBDoPW0NF18iP2h8%2FvAUmxLA35%2Bb0LP4r2nq%2F%2BNEZofkSvsvVRd0UY%2FjmoDwCAP9zXu5r8GJIplZ%2F6%2B2mZOYOWyRj5KMjUeeRbZ1CF8pKkGLMy3j6bgrkm5HJVnaRTCvIfT1Bs%2BiMz%2F%2BL6JBhORnyCTTickAifNXSLLrviAfQ4eUMZbh6ScPgjYhMp3t3aJJ871G5Ajae%2FPJnhOSDeDfdmROyaYznYFN1KY5Dbx9NZ0bnHHlQ12oOr9jS5ngjFkbeCBUDBhHiM3Uku%2BVAPH3KHrzWo6dc9NsekLrSpjYUktTpx9W8fE9BVymLXrWbwMdSCACECQ6WiBfFE7jCYw5nIBjqkAQYHweVxycOJ1%2B%2BLLPAkB3AeMMR%2B6jOigy%2BkV9R1Nzu8SpcIWY9r3Xx2CLLrwxqd8PnPI7G0ze%2BVd8rX1cL3s5UZJgcHutzF4YvFBxUIzkeAg%2BxqH8mt0RoWQiYjrOqTCK3Pz94n4HFLig55TwuIGFfOAL2S68%2BGLYklBgtqPDBXbCQGvqj2VjO%2Fr8RukFKteb08yMNOnNcN7e%2BWmKJLlEuips9B&X-Amz-Signature=a8407ecc7a7b6901b0b064655475bceda2f83823acff0a6002a9fb64b42fb951&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

