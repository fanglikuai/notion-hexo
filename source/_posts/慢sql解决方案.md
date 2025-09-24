---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EY5VQ5E%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHbenExfNHSE29yjRNVWme2%2FEcZ7zBspWVMezTq1782fAiEA%2Fzw5GTy6Lo4kFOHVSELvajPYiMIk7tC5PII19Hsh%2BIUq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDB6UIY32gs41bULLZyrcA3oB8%2FMpK09W65LFB8K239lEGyUkV7kkXGz37xQl06PRtZ01NXOmgWXmmg1F3iXLwzdG1SZ82p2IXOIYS3aDqjCvdFhiORtaJZo%2B043whc7ovd8FxVu47PmTLb8NJUiX0k8%2Bfwqlpx9CVgdupsvLVQTmDIcwRChy%2Fe8QLacbxhRAxRdaS5TfMBiuwe1spCKSQOH1jaMpua2mvl8nknQaw9kKrffsgyl1xs%2Bj6LzABdo7QIWIGTKlzt9b%2FQKalYV0Nv1F8rg3biLH13sn9mEP4IotpEGXeXgg%2BSkhn7%2F8gO1q%2FzfDeNnKgXosCtmRylG46wtv%2Fjr9jqC346zkLfJKz25f24TRy6UUb44YLLtjnJHMQC843DSuYM%2F8kznPLbYZSM3leh%2B29%2FHLKFEtTBA7%2F%2FrHyeCt0rlbDU6UYmRSIjzXhWkYq3LDQGNqhlI%2B5gzWYXevzyxculxpv642MK8xDtonK3RZhg0JsMiFNZUdMiYWaOwY5kkEqyYm6cwmAzPZqKOO9VUne7%2BhAJUBH69o55mHTa4mubWFoqK0wkhrr%2BPKqNMrUwXI5iU4kob19IFJy0obmGQyyb7B6RZuyYPozPW%2FEN5fuQIHPZzBAWjmh7v9HviBBsuLRAipksSYMJOvzcYGOqUBN%2BU%2Bru5gp0Z9yEgoqJmXUqm6tsvWL4nCQybNbsFdncd4gkaWzF6l2XoOrw%2FVKRORS5v4We6fRKb4RcsLRxYHVdlGpBnNB57qp4xFhhhWXRsacwyxJBfSggshoKabERLPQiq1Sc2LcBzrSfTQzjbl12tzNLd1VrgYJ4TSt%2BDUSj7NAo3sIKBW3%2FLT1zFxhYqIO9Me8FXcK%2FFB042BGyZLsBqvHnLd&X-Amz-Signature=7005beee05f2eb4895d6beb3db401fc26783f16f92511d021b5966e9955b1590&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

