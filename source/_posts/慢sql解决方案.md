---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THIVCJ4D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtclKrU8SvaMU9M00jjzFOXRpFudG1F8OgrAs04iy83QIhAMOFLCYPh2Z%2FmAn%2BY6lssMPQcTNgs7SpyrXPRIUpfK89Kv8DCHUQABoMNjM3NDIzMTgzODA1IgzrUzLzUAK6at6yK5Qq3AOPVCSiXmhEBCSzA4v8m%2BFrSYXJpgtgSc%2BDKkXP6BMD1bkw6h4XSQhdys%2Bzx1Z8JEDbVcMoWLtaWPtEgg3NDU4M1ajxtROocCLnf6%2Fl8fA7CayDheXE1cqy3FWI%2B6gHOESZMWrniV2JRRpRjVCLOf%2B%2B1X5OftHrYP%2FECHd9O7ZYbNJMq1FFt2EWzGWIHM2nXtc9eBXdAH9gqY3ySGZJZrm5nKZk4OtjLGilU18dAJlTyZLUC2FBdhv6Q7IQ1bYL7c4LuW5pdSoEFyDbPIcEAGc3dZiEM5%2BQR7IuNVeSrPtkkHihbMdTtN%2B3hUUAH74q%2FQyj%2FrEzAB2mn4krPu5nxRS4JylSb64Xb783q%2BLTBE0DIFKnGBeadNeZ8zuUAeyEqLswXmCEDEksagVSIRLqv5T032jUSgYqbrgK6I53mxgsueEGtcNf8hibcJc1weKnMCUqZASooekjgCjWjKRVbxKE1CZSeJ4xHEs%2B%2BDTgZ7DiRqVLupaJHLJhFHVP2YMe1uz0R3ZemOrNR6%2BqoG%2FYHHufqvBoWkK8%2FKNqHa1DrAm5UHg9Ve0qp0vlDFBLvEqWobDPAeUd3T0X6elSQBiOLpEPREFJRU%2Fp9gAz4bE6JIEM06qiA9nNxUJURwQCGDCT0afIBjqkAaUGP%2FkdKT9TwDvo8ZG2hCiZRgfM1tVlep1u4093WPOFi6RlOvqBdYtXA4k1Gbmio4Z53JymAV0JJPJRM3VsSxIdH2FuMS58yarMpqK%2FIRosDNwd%2BiCC%2BgbOnDKocqCYG%2FXSnqek%2BqXd3aOrEv1uZZvYwLhHK5MsWj8G%2Blx7Ad%2BLxKrJFoI0ggEnV7SmiEJFJ7AajexMX3PBIB0xtZj0o8vS0NGb&X-Amz-Signature=9f49497b74b8fe152a1bf3c2aa3a2337ed53d63e35cfb2c341c9f177aac62302&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

