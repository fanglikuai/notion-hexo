---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTODG5D%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpVZgx2XxlX%2B08fHEhlk83medYXiVdQ%2FWQ2OTYjFtAzQIhAJ3K4wecIpkxi%2FAkTTosonoUPfYSg3jBQKhNXUG%2Fb%2FWdKv8DCE4QABoMNjM3NDIzMTgzODA1IgzGZ8fz8PFVJD9DyuEq3ANV1vvBRSbpx8nJTcwiCwiGTdu2ssYKIGY1ghOk5hOzhyDkG212mJd7PaHNMSnYTm9A9lcDejmXU5VPm3P5TVmOpKd%2Bs9kI%2B7cePyWjCgZBdaOdsQShJCVbNdsTWSpwmS3QE%2FR%2BGo16KOM1ychzMudj12jqAT1YGWwa%2FR%2Ftgxned7XIVpo5qeb84QXegmlnfu1lzZhIuNQrDkBcgoCQIwha3yWe6WKtUF8e%2BR3A33CKK%2BmC4cqNSWunEBM5fwtO%2F1elTeQaqWH1PnXpFenxitiFg7Spw1LfO7Mxnf%2BpGopoWwj6zGvh5%2F48SPCJUE6J%2BrmUfZ4Aq5C%2B9FrP4GdfcWuYXumS3BCkROOphOYj1ch%2FxNjT62fzDVDO89oxzrzU9GBEJTAGXktLWamrltmiUD8T9nD7nenxzOV74GzjHjVQC8Xx7gmVMlpfc1mt5iR53XsNYhEPuLfctEi%2FVkilvuMh%2BDIR2arjf1phthGYoUygW%2Bnbr4g4fWUAZybbcW%2Fs7WdNJuaQuxg8HOqCoQU3L5yZacpEIJw6iAhqSShgfdvAJP%2BAId4CBqfZXCCs2H7PsGHd0rrrDg%2FYX5SLotTU3ziOv%2Ff8Xv%2FAPnN40GM1u3Pg6qn5DMCh%2Bm6POjeM6TCNnszGBjqkAck5VSy3rFoju4l%2BMPqk6At42%2Fr%2BsHQkZRqyTWQcm0UYwvh21Wf9rN4Oi%2Bflftjbkg1fQY%2BDLKJrH7tk443%2FTH3y4HJ9mk6jrgmp6AEPxRYXYqyRrTDInZGp%2B396uQs%2FHMkcDtmIAvJex5qPMudmNzsodrDqBcvCRa6v4kqch6dXWOqoInF2%2BvzQ%2F1UQcupyZ%2Bl8BIkQkBqUzhsyEKFue9IbiWR9&X-Amz-Signature=42b7e9574ea09a2e308ea43068a12748ee9464d94e6170a47e6cc54337a328b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

