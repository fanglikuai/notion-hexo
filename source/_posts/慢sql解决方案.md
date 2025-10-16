---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDUVIP4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID7bhHLPeh4W8XO02GXuhhlkHihxISYhblblvjNT1mKDAiAWz8byUFajbe3fp5szNOYzkaQr1pIDjDNOEyTvQKbvlSqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqwf3E5qGjyjBr6m2KtwD1LkDUmXGraNZF4uBbxV4w9z0nVXd8W4BoaJ4ZRXGVMDMy7P6S90EEMea8XaQ%2FA2LVDaFc5qXJi6ovMD5YSFPLFxUejD3y%2FlYqgjdbH8SFRyc6Rz6SwfCWqzzw35owSeCRUT6s%2FiTzdRxCXob9egvKPZGhkSaagGd5RDfYafUo4xxM3GBJzW7cVj61fO8%2BxDRhDdWa%2B541OfENiIKdt7vbtkhJ%2F3gDBC%2FkTrOCOa0q5VueLCtBPtLuT571%2FaNg3Bq2GDFSCDtcmRLcuhceFjpWC%2B2w%2BQEkQJpCxcuBZ1r19mh2%2BNx5xpsOydXUzbHwP54H3OeeNQSqL%2F6U1C9mPFrHKL6bxz4DHZ1FE9cBKIWwiGOoy%2BRwBmD%2FXI4Wn%2FHavwXXgg7abEd0B455%2F97M2k6Re2HlY19N9vlkP2oYx66rBrhBFFnK7xPnQZVBmi9ANee09SymCSGy28R2xae02UPPUpFxqeBKFktJwDvWtyiXiC8K5syCl3nsE%2FQD2Chmn52Fr13G1emYs3cyMkvKFermA7HCeIM0O9QajB2T8y2K%2Fbb6%2F2IuC805KGvixP5hj5CkcKR%2FyeMnUlbvfJiHfd%2Fd%2FLvPcz418VBwERCyUawON6pEPjBtQP7tYBPyngw2dnCxwY6pgFyCpDF%2FNURB%2BS3Ogqude%2BUgE6eMMn0pCfSPY2mXyP7ISW3RpWF9evMuPtmIwzRbkbsB7J4RiIrvJH0yKsvkLDwv4nyiKP9TVbp23eDPkDBc8lG9KptZYqyAPq%2BnP%2F%2BTFMy4tYn%2FhAxQW0aBufGXH5TMzRfyyFdnOS6w3d59cfKHN1JDz3Fa4tts0rcKNgNTt022M9jg%2FoTCfeNKKS%2BhlZVW31zRaW5&X-Amz-Signature=bcf9039b319a65ca306df19ba78a59203932ee191d57600891bea9bc10c46245&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

