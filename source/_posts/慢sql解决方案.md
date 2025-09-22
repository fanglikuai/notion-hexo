---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CUSITYP%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHPljDJn6I65VXnuYnVLlj9hVCpeJ%2Fn9OLYNHc3%2B5W7QAiEA%2Bs%2FmA5u4cr6VVcHq1eFTohsy0vN7DCfEXNIcsYhQIL4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDKa2T1LuHXlMsluNACrcA3lUkstnzE%2Bk62%2FJ0GTAEjhoeH2mHHjoGrHse8xFK%2FOyXKtceG%2FB5yZuZ4eRTa4xoY50E7aDcVmhUsCDivVeDG9EAR6zpgzlXL4aTPgDzNadbWrXiwZeYl%2FA94POvZxUIdQCzX3TX42UD0lh2pmpbFacmo%2FL1TqrRqqsAPvWagmJSltymVAOeaCuz85%2FcDpZ%2By%2BT3YDfpwyYLYkosuRInvr16EjA35T83MKO2VncCn0AqaQsgtL%2FfN0MRiWSx%2F1vTnjJRoYYXArVc0G%2BxoFnGkh1UhzPu%2FUPLgN82WTFQ7tZ0pxXIRecsyBgR9KtklncapZQi1prwPNiRQdiqYXaIi%2Faa0P7jHdQgmjoyxXvQoLQ5lHFX%2BJEftTJCdRX7pfTD4Cze%2F2pBtv033sOCmup3w5nx5rHgkQM2J7wK0dPW%2BL9uY4khZD8u%2FIAqczokkwY%2Fa5gWwIX5R%2BwXSDo3y6ndNPU3UHLG6p0%2BZSznBmlCnqPmF%2BcTA66VNwicsaihmz86LVXf%2BQJdtzD%2Fb24aDM20aCZvPwaHNfpJJZsFK%2Fw%2B%2FDVSbem5AmsnNO%2FRhHy48fUf984Q9Ww13r4AiMzcww2Hda3vvAyJjEUVztMmUs5Odp5nAoG7yY%2B76kVRSorMKL%2BxsYGOqUBnzbBOZeZZeDRBrHKq8U35nD0Nkg4T4ZW5jn6Gvg827zFVLHHgeWXUfVfSxhrUG8hl274KNOvBrJQc%2FMQNweG009E2k5wi7A1dCW5kttc0NOGLjWGxzSPGSQCv33dqeyqOQpJ28oVr8yUpr%2BddVrAcIJ5tr0%2BCekEkztmjXoZe1zh5AQL%2BNwIJxwmQ73GT9vzWOJmAALclBhA3nnbTFbVBBpPQfrS&X-Amz-Signature=eead7035634d5a363cf2dc69dc5931013333632d46cb4354cf0d01b3e5d58435&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

