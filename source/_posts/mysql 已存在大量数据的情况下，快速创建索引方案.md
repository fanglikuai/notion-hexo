---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HABCIOW%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4pWlUhR29xWhshMGLBJaFQ04r0rGIqWY%2Fy%2FS7865cpwIhAIXZTsBqLJYTByg5bOMN%2F6nlX%2FuBd0bOxxcbux2U92nsKv8DCHYQABoMNjM3NDIzMTgzODA1IgyrD0tCL7AKjAmOirUq3APK07DYWljlGDJRAbAHSmSZhG5LkvUrL8HPnHNITrKgOOYlHCroxy75f8CoLAOY4v9XTsFUOUtu9vkAI%2FbhdsxoK9u8jXMwObCRPBifGby%2FSirCdHOPBnI80e7az9cWxdbriJZVHOiGkMScj36pMeApzNApxScuUgr44YumoyuPVZR8agHHpgo5f%2FaVUAJJNw5usn1yHdw7qfA6g9Bm2%2BgFVlA6WCskUHzk%2FyfztnCyrOK8S4XRmziM5yIVHgJpfgW4xvfLAD2vluMjRW35tUfa%2FmfAt23996mS%2FR%2FSXt6CFG%2FUXYkBJKz5Ys6ZR663zIDcNKBiPuvg2tGXKG1bViixr%2B7qIjZ9Dfs7e0wj8Zjoiq22nWitFrKxHcSj4o%2FSiMwLT2n%2FwqkL28x5b6dRzdGal55b03VEtRpdNhy%2Fk1lw4vVpC1yEYF8VQS%2Fq8NVLYPXJZtuPdXQdF1PSTmLPEJEMxghs0dejk3kayEDRVcH3FNVc4HCInBSIC%2BcIJALAeGq8i70axLtZoPGNyhahNGGtifDlVGc3Sk%2BRw24MrFZvusogReO4ye%2Fv%2FkcDoEXQcqX%2BTwUwJt5zuZoZxmLflzzmx6BmP77T%2FgDdD1eeGgirmWV0B7tv%2BJZIPsvkvTCg86fIBjqkASILzhhmHf2gWPjbzKdS%2BDa%2BY%2FCH%2B7ZqzkCjTKiJBu3LEa%2FpwhNxUbBWVy%2B%2BD6KOjr29WA1SgfSq9fxeYX9c2aSFSfNfeDZAOaQ44aLjqpkmMLJuoRhztyi6OGGq1UGUig2xifyzMwYZyJF64N%2Fx%2F%2BUvwAZPYl1C7lJT0I%2F7%2BPi72NqUlylHbAUPfSEgcInQqEOk1oIuX7obPM2woJxOprWbPOWw&X-Amz-Signature=673ab93d149df3db0922c47cf40fa38e729a0e30396eeec68a9cbb866b8bc631&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

