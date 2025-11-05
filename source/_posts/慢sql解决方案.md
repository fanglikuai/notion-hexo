---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XEJ2QJTL%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3L7%2B71515DZxueFKLhNE8F0ROeGXXwZh%2FC2MG0NveNwIgKTXwF8Kj1wBP8FQyei1zZSiUDQpRYzcgjaYxlmGumYkqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL7jnbKe9m4cCWsNEyrcA%2B3W4OVrbr%2FzirVjweOZpMNbDz2madc8aZEKX5LsANC6wpLd1o9uMHZAdPa4nHsWrB5%2Bz3tNLxAXntwuX%2FBJ46KPc7dgXsLbMS%2Bb2yrYHfhPWCtEisH%2FMjnda7okaDY%2B4A77BKaRTpQWq04M7R2mSgB4PTUSzAiO5cvgS%2Fm1uSSrxhAt3%2FZzXbJqXhbaqJZSq%2Bp6ysGeGcTaK0ti0PXH9FXeaX8fSFzR1GqeLONqNJqP2luSMlPmiUlstb83Ww3u2sHEyGaQePEPvddSn4jbXgs9HmuU8iYybJpTWf6T0ppKnPfMSIyjOs%2BeO1sK3mXSNbJpH80V876gZ%2Bxm%2Bbu8KgEISUfimgZRdGNTwCD8zxECk4FJHIew6lYBq9SXZ15NXc3%2FBSxIxHfzF7N8ONkcbZzHFb%2BKXk0KqUpYmtrl7W33V0y7DlxeO34IpJaIYCqgv0GfNcGYiZOpo8zcXuseouftCoJzeBUhPEFDixROfrdE7FaWn9hF4WoT1q4gYSxoMutyNS0KGY5OtCas7l%2FjuJb3d0%2FpROwdKk7qxcRwuG6mLvuiwWEHINzlhSW3cmO88akcK71VINlfODIidnI0RtjYV3ISQyFIu0Ur8xFSozfJFD0nKIGALgL%2FHf2fMNSFrMgGOqUB9yTTAEQEwBpob9pMmWuuuHRTOPpneXgdbU120xrU%2FYeLTebcQ8V4GdZ%2Fj%2F2wLSADP5gSGeuZUQkHRUb73j4fuLqVBpEnL7wovXxoxuvqsPwcakMamDeTHEGNoEVANX7f%2FS%2FXHi1ajM2XjlZVDmV2IH7bJKNHHsrZphhVGPStpDfnt%2FY4qh%2BomXI3ziuxq4%2FxSsw52ZTAHY%2BfTLGjn6tonGJmQY6u&X-Amz-Signature=cc976b6d772043f844cb61d0739f1f43888aa6c9a7187d54a8c67bedc5753e9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

