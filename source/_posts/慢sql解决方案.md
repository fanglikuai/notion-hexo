---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DRCXFZN%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDJpH6YcO4FjN6NF7roX7lr5qun4b26VwOG7zsHaZFh1gIhAJn6%2FJFCVcZ1IiWOOm1qA1lt8MV9Tepr7K9wtF1%2BCNKuKv8DCHgQABoMNjM3NDIzMTgzODA1IgypieNxNHSlDbiVb8kq3ANU%2FG8DVc4QkpIJubD7DiQA2fqRzi%2FgWRnMgj%2BgAkGfXlDGELvGdrONh5FPhU3iUAb1p1963ZA8QW0HYX5rAXRMEjKtlEcAC6p9iVaxtUQ%2BfZI8SDzPSLwHpHiwt7SYs3bU6ESMe%2FKKuGMFN%2B3JhJIOqzpymw14f%2BPN9rXzY7bAM339T6dCkgx8n8LEhb%2BQPQTxnD0W1ZNI5Tk1oPGreTnyaeJXUr4a0FHl3MvE%2FE4j4rCAN1g6%2B%2BE4cwYLo8x1Yv7TeapY5XjCiCUsRj%2FjkZQusMB0fTMSz629ZYApL8r6hg7JyXDCzD%2BGrr2S3G8GhzceFSX9lYPsXnui1JoO79toZ%2BWmo%2Bl5YEigVqSSHCQoK2K4zTJIqtIzrwqGh3hKORsj9DyY%2B3xc%2B%2B2EUpOLE6351oBrd2BmHrGqHml4RMJCFOpTxamSN5H%2BksNuyE9JLfs%2BDpRnEM9qR%2F3G%2BNTBYO5IxSFfCAZaeyqrramwsXyc6EbGDibcWIeKRiZwet8CE3ZPz0mqhOH%2FZhEhzEW1vpCQHmjK3AlsPDDqwRlg4NTmvxiTsZjch7X%2Bmo5J3VM8Z7agKqjHzNVivt3Fm6Xv%2FNebH7Ncqf9SEALcy2EH0Z2igCgqfZEhh2d1TA%2FavTCA55jJBjqkAQUox9ditAF7muuqvQsN7LgofX%2BUMu9YGtPQHQuEubsfF45JH9pyaomCfTQXqGHxixNzT%2FQlTgRdDSO91Mh0qMUDZwuwVbirpaeR%2BfGKIXmLTSSPxmWaFTb6k93zeAL3dGLXEZidFL7ef01St4lyivKU4ZDLyfXGCUOveiLNDK2HVIafT2O0Mz41ZvORVQ1PQPoz%2BQ0VacSVRZuyFEtAiKO4qOGY&X-Amz-Signature=edd2673e85bbf98d948f07048b03e67cf3dbb5f09a7a2bd3fd4b8cc5df315335&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

