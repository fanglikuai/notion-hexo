---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HABCIOW%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4pWlUhR29xWhshMGLBJaFQ04r0rGIqWY%2Fy%2FS7865cpwIhAIXZTsBqLJYTByg5bOMN%2F6nlX%2FuBd0bOxxcbux2U92nsKv8DCHYQABoMNjM3NDIzMTgzODA1IgyrD0tCL7AKjAmOirUq3APK07DYWljlGDJRAbAHSmSZhG5LkvUrL8HPnHNITrKgOOYlHCroxy75f8CoLAOY4v9XTsFUOUtu9vkAI%2FbhdsxoK9u8jXMwObCRPBifGby%2FSirCdHOPBnI80e7az9cWxdbriJZVHOiGkMScj36pMeApzNApxScuUgr44YumoyuPVZR8agHHpgo5f%2FaVUAJJNw5usn1yHdw7qfA6g9Bm2%2BgFVlA6WCskUHzk%2FyfztnCyrOK8S4XRmziM5yIVHgJpfgW4xvfLAD2vluMjRW35tUfa%2FmfAt23996mS%2FR%2FSXt6CFG%2FUXYkBJKz5Ys6ZR663zIDcNKBiPuvg2tGXKG1bViixr%2B7qIjZ9Dfs7e0wj8Zjoiq22nWitFrKxHcSj4o%2FSiMwLT2n%2FwqkL28x5b6dRzdGal55b03VEtRpdNhy%2Fk1lw4vVpC1yEYF8VQS%2Fq8NVLYPXJZtuPdXQdF1PSTmLPEJEMxghs0dejk3kayEDRVcH3FNVc4HCInBSIC%2BcIJALAeGq8i70axLtZoPGNyhahNGGtifDlVGc3Sk%2BRw24MrFZvusogReO4ye%2Fv%2FkcDoEXQcqX%2BTwUwJt5zuZoZxmLflzzmx6BmP77T%2FgDdD1eeGgirmWV0B7tv%2BJZIPsvkvTCg86fIBjqkASILzhhmHf2gWPjbzKdS%2BDa%2BY%2FCH%2B7ZqzkCjTKiJBu3LEa%2FpwhNxUbBWVy%2B%2BD6KOjr29WA1SgfSq9fxeYX9c2aSFSfNfeDZAOaQ44aLjqpkmMLJuoRhztyi6OGGq1UGUig2xifyzMwYZyJF64N%2Fx%2F%2BUvwAZPYl1C7lJT0I%2F7%2BPi72NqUlylHbAUPfSEgcInQqEOk1oIuX7obPM2woJxOprWbPOWw&X-Amz-Signature=7b2ca6a937a7856ceec133c4b18297bcd52be71cd24aaad2218305bef9e0e047&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

