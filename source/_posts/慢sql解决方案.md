---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZJUOTN%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T000036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvQQ4o8N805BqL1GhU2LrvMd%2FPt1Jo0Ry4sMUKp%2FgD0gIgSgmWlE9fNVn%2FcgO7iz27ExB6wRjQcqektJuGlPO1fskq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDK%2FfwBggwz%2B7GgKfEyrcA29E08SnPCJYDc5SHn%2FwyBczUxRBfzst%2Bz4BC6KgTrn%2B8nR3J5CiUEy3BSJD8Fb0Z4HMIh4w0aQi7SxZNej1dYOWvVlHoYt1aGMTH2r%2FNdAKJreYI9DfkeA6IJLROF1LlzpYQGtT6%2Fln7JxXRFNi1cePDGnMG3NOeAlot9%2BcNRifEOEh1NiZJ6E0b8HSo42ygbm7zPfZUKuehRyc8WlPnjHGASJ8gvg5UoXFrGYZjUGTWNGVHsJFcf5rN7vk4US3nl4RRGcwxVbVUBX1Esw01dkZovgVcn1%2BHsGezRXKqRFnGyl%2F45CKYmoquS3Eh6o1R9IdwwwH6Tn2%2BbK8CQBoATk6DsXq4%2FG2WAUX22GYSq%2B3eGjBlfLoO3rxbBo5uJQG9hUb2oEUQ4AHaoKmeJ48lSTnybta4R50niNokL3mO0bNCWjGJJWhXmnfYYNeyXRPdVTV3%2Fu4dLuMlfuj00fwPB%2FczkRJZnJ3ogxi%2FgpfmtwcMsDoyxW5i86BjjJgDbOTolCERRlJ1A%2Bh5VTJYaaeKs9h07b3Iwb5QucbSsNetRmeh4YO8d3rO%2FDPfsbcodDoE7%2FZm5I6G2pQEDbqeiJuo9v%2FCM%2FNw3LsqM5bBm%2Bp1uFrcWz8wbqVLgCuDIgyMK%2FhhscGOqUBH0qTMoNCJLt4%2B22qfiwIaj0ZBV4B3m2VfS4BLeB3ttzrIgCYSCXV7va64H3hPX%2Baudt5URdbtLHczf%2Bma2%2B%2FaQNruPHBBrbEVx1Y62RwBi%2BDFoEIoOuv%2BiSOF%2FjkhymXfaUbGvz%2B1oGyvvLG434CKKdeNFAn9HtSVKhAyulpaNX1vAjSpwOfPpI4zs8qXgyewImxYpmdx%2FVORqy9sYHIvZAvN0iF&X-Amz-Signature=b559e7172e7473fdd5605e1a51c052a5078d3ed8fc15972c57a1279963dbd4b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

