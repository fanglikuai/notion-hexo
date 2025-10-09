---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHHOA3FS%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDIRUB8nYZYEyKRVZ7HFGfRArB77kGD5WoLnjqX13zdogIgJ3UNSqiA8a68DNYoadkZIzO8zrJctQwV4ClO3%2FjfAmMqiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDESWESoSG8%2BGS%2B8hBCrcA1U4Pvq5YYUdmRRxaDXDHQk50IWcleLObckLL1%2FP3ewqKPM5hcq3ji4jHypSQnSQZFF67z2g%2Bxp%2B5ckTxn1Raj9khTQM89SCmUnq%2FKgJQk2q%2BBMokuewy5lKJWp%2FRiNzh8nLJ%2Bq%2BjLNeHPGgcKIdQuJkQGuou6u0se9yBJj%2FtHKkt12tlF0EMqi4C3xe8qKtoug8J%2BhipiVn1%2BozE42%2F0d6MmeKqzD91T6%2BCtt%2FKdEWeolProFDKUEvIwulcEFfmzDnLnQBKaIf1M9LVvOCv3uQuaY2VOajVqjCftg7oJi1gy5XTru3M7BmPzzV%2BO1e78t3pzOKPDgfOVfUCaa8RoMcqC7TEbwH%2BerE95FN99%2FCVzku5EwOG6EOYKt%2BG7xdMpMSuHNElextxRFPZtG35lGThZ96TqSmoVy5U4vDd%2FfYO9lKTq8Yg6dlZ5%2BGb2oeKEVZkdFgddUEGKVS8kTzJsFjrNrcTuE%2BDwIiPLYpha6bXdA%2BPkIp0utuXbQzBCDXckNCi3UbljQZHjJXbRdzgZj8jNQfj9t7%2FLj%2Fy9WtJkXG6Mag52aoWd0ZK89i74q86zrfgafAA1eJIDP3u2%2B2Rn7NT872YNAMYtEvhT9AZRdn%2BGg1lfDY0AFpM4jhOMLycnccGOqUBOOvS5UFSTebw1GCuZJoXFzDBEWBZEIVoQYK9HKIpkT25jxJzv4NYxwSsjMd8qCUfxW4NxjVmS3K0EfN7HKiR1V4Ko%2F0IjoV3CKSb%2FEIaMSlXWaQ5cH4SSjtNwtMfI1%2BIWJ%2F3%2F1txKNv%2BIE4G3DV3Km9V7zKkdSw1utag9txL0e%2Fa8ia9AnhbCvDSfrAbrPzxwstr%2BsbBTMpsvagNLoetyww2gPrH&X-Amz-Signature=b0bfce6fe01329d4cec85bd16f6f2bbbe5326a3095a0ef488e6ef19fc6b75537&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

