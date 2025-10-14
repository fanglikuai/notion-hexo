---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HUIN2M%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvZGmo4Bv5M1JqIcl0qmFkv1dfRH4NzaVRxkQyCWgDJAIgJuEdMuWc4iRx0PYVS22CP8Oh%2BtcIvVOtfHVn4sID66gq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDBnaLklzKPnEMs4kkSrcA5%2FcIEi9ONE%2FAEvqYAdg%2BUH9oF05g4lEVM7Mz%2Brj1f8A4VErsjHwm8ZcSCdr11GU1cTbAWxhcgzBD6M2Bi0vauU2AWB2R7HhnAuikEEJ6vNr2U9kTZeI6XUCuKcfMHsbn7IFCnW%2BbIEr%2FkJax2JgVarLS0SG0Gl6JziyS8SQsrSBBSZo%2B1ZnSn2hYAPawXTgW0TADnRSV07liwYHlsmpMfemr%2BWoxhbhP67uVjcYbQkVPIPjfyfSZg%2FOevp0xNM3yxb%2FWc1p4i4V9%2F2oz25TfGy2VK8%2Br8DHzGlh0ZjTysa2xgvSEL%2FR42lM7WGhNusnrhiCc2puF9HnV9wBxvN4RGYIyAzs%2F2e912V2WVTXSGgj2Sov%2Byi68117Lm6IpM9TJdfkRl8Ka4uqMO%2Fcx1pTFQkcA%2BC4jqc4jf1HW3N9yt1BRXOt15OiVYV8Fb%2F0M7S7CJStnzrtb3G7PR3yBnIfKSoAcDozJ6uSuyN6kT8EMWU0bp0t1Cgl6WhVaL%2BPJLFFUY45zj9clo0M5Se3y2eG8xhkyOPMrhrWP4qU6wAXATTfeP%2BxmZL344fIs5P0nWYGgdJtnMO7kA%2F%2Fs7JBy1fkl5D7JXx7g9Eeg9W08Uyf5boduzdLKYoSqLWB4I6sMI%2Bct8cGOqUB3WUgSN5pp9RiTfXH5yMbhlN%2BiZM3buvbq%2FJ%2B9YEmU8cMnnsgbMSiTdX6PPnY%2FZIV9hQXEDXEvkwpdDJr%2BIGAhokaMCjdNRo1oHQh2QseqyUR2nyIb%2FHXw53XVK9%2Br3pze%2BacBCvq7c6xZvrXmu%2Bq9MjzdffdW9A%2FuI2vYEeiq4k4rxe8slLaHsMCtCNZK42Qq85F07A%2BZJWBn62%2B2V%2FPZqsCfPXo&X-Amz-Signature=25b850c8a1bbbe5ede159f5e6691761d7275538737535b9f1d5d77610a103026&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

