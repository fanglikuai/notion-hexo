---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVGPT47%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCaenjbkvNrVvol3ngA%2Ft4cgdb4dPZrQkbnTfckqsn6qAIgUM%2FKKalYTDwTGkPXX97FV%2Fb6bEAZ2Ltz%2BlT6y5Ou4rQqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIN2cCWoxAHM0BeVzSrcA99DkkFeF3%2BEk9LpAlMmzD8%2BcRCbJUSoBodPGMpLxLZIhVJFPY3ZKSq9NGVkxu0QQQ9A8pDLjF5JKK4O0ffIbWTKoHdka%2BJvvGrBKhQb16A2zk8UBIyU9%2FjpulU1bS8j18KqJw43vc0qg%2F2sV7fvRKqZdR4QlD3GVp2fcGL26IaRD9cA9xxlsuqj3p%2FNVumIjXKEDgJGcYOgpLUe%2FkXLlweilK39jECnEGLlNGBYdV3A%2B09v0wy9NnMHVtnQ2sRQpeEsSSuoQoGfxyQcPi8msTF5rJff5nyxa6Sd%2Fs3y8XCy2KcjyG0rxSLvhxzt6oEppKxlj8hBmODAAAtB04UA1%2Bs5mPuKetgF%2BixfFpmbgZls2CY5WSpwkw4S%2FrLx7mSok5TJdlwUuhxBU9De%2FF2uo7mEcRxO3%2FsMtbzSZw6oy%2FQO%2FyctPeVN4lqJwjdqTyaF%2BMz3ztdLFZeKtXknJnVkHkevVnETeULCeg5BXXdfmAPoQNeLc7eLTkKyhhAi9MOuOgTzZNhMnnu9XF7FwZeEpAXSFMTqQ1HxZ2XP0jh7FXeTEaeJqV4yFlzPLni44zbbI%2BOOGWM%2F2PN6qwtgiVUVYGYdPWmBtpn%2BFnzLsg9fJFNaI1UzR5nMOx9SaCGuMIbrmskGOqUB2BjBZC7g0djdgeNr0lD1RVwjVWxUhXqP5Ipl6PbCMlm1F38RJxpPFwestXOY2ICL%2FNV6rpjwyoaAAqVVShiY3MuWl34F0P7m4Ydq7Q1CpO%2BFay%2FN8KxEBkZyJrc0eA1NCjb7tmFRoRsrSS1slciQzAzoFGVLje89hXyqnvKwftOsh9IWqVThh2%2F2jz6%2FtH7%2FhT00OXqznrzRX8T15Y9V1er%2BGQMJ&X-Amz-Signature=516935ed09bbea17866a4a6c0e16ac15300471c91f0cee64841df0ab3f4678a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

