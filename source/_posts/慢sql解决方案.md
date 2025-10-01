---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQDSIY7H%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQDa66qMcwSlMWxzoFb%2FZ0elNhlTsRtzZCDZMtfr8k0TgwIhAPwjwCNQsTqqI3g3pkcerbdyPUd03m0nfsokC6orIRFpKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDjIweP4yynyv22g4q3AODz%2Bel0We08otVcX9LTy%2FOwcoPY5E30TXjaT2uJKxxHC%2FdeeiAUoR%2FbwDAVqSJFouvdvlxpG7Q9XcVT7jf2pPoouDscbvyg99kDw8%2FilAssz%2F58QuCXmqalIFY8UMuGq%2BTSKMBtOfmSTYC4ZtvKZBIo181EfnvTqPxGseg6NmCeac9civdgUbHb0IDrEZRMskfqul0h7JODp5ftM4Wr8OaeqJGuvdmqG2axWBvDx0zmJm1MX0apuauvnIS8AiA14cemeEl2Wm%2FcfYSLb66A40Ze%2F1Yl3u78O6pVHOEUQxiktIhfBz7meaW4gA01p8qjgG6jgPmRg4GLPkFAg8cwsR6ej0KTgQvf5U5h0Ig%2FBluDgdmdRvStd6LlOBpz4M6%2BXSL2lPI2YdW7ZVEGpTKTRdbRqsZne1VJPCFqJ9fpK1uCHjHwBMBK8Ig%2B%2B4eieL3aaTMdQBgFl1iQlpNGxw9ofNT8p30xP8bf0EWjLL4XWArCoHWM0tNxNsWtu2L0SvL%2BkZIp2Hb9EgyJ%2B%2FMNxb7G%2F%2BA85KdGCG7ilmf2mHs2z0nZMPVyi6S%2FdmwHyGQhY8mykcFL7v3EV6EWWDWhGLdJl%2F4aFcTRRjxso7XF0I0IUwwhkeFyZi8JL3ZlqvNgzCiw%2FHGBjqkARBKrdZHNjg983SnaELnHpASpBS%2BL2RnHVUJeTX5ovSHwGr8Bkq%2FjLi3XJzg8pTGpdHRDTUFKM6SbtFCjWacmBvKNQC%2BqO1G9Mfi3iRiKa7c6MpSmTxBNfjbXfjqOKyXPGRJdINlwr8RuNEKqx9FibJFOHsdUEJSJYo6%2B5BSb1vy8rQi9X4bRb%2Fg17axm8GrjvOt0FNT%2BQ%2BLZoX%2FwukfAMN8tN%2Fi&X-Amz-Signature=f2ae69900d1979f005805379601f9d9ff3d6501d5f0e0751e937cd7a42316cf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

