---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Y7FOWEW%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJGMEQCIGE0ylWi7ifEERfqTWrbmPHMf7BkGuoQUYJ7VjHmKndAAiBcQEg0jbE5afSJiRka%2FQP9J%2FOmUgWPmQV6TzRsqkm0dCqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2%2Fyef%2BWPAIH1NQglKtwDrO%2FMdsNQJ2ixtfznGe3o9h%2BFIuIBZQjgtHYZbm%2BBINpHI6usB9qMUzFShm8DJVx5bXwU3AXl7nTe46%2FLNrEgsBneYz%2BBHaaZqi%2BKHVP%2BTmDUINxUvfPwjtGJ%2BnhSzTEZrjxNqIGfw7P6pNg0EXZGuI44yB1s85uBZusyHJdrTwAt6m0xLJUvjfFhDSP8l3HfKqGLLhb32KseninMSeB6A7RYdA35BuLnkjdR0%2FE0HWOOcEENKYncizrGR3nNaVbjc%2BnzgjXJJSjkcpne9aFNEV4vKyTthhOVUuMzCsg2a0oMS06B4arEklU365Ci4sOjHbpGbuvfTQ74P6VN6m4mrRMb57zMJbq8qv5yH2iJczJ%2BHYZ%2FWvAr3iXfZHRawQysGNurvWt4p9K6Ft9dlYNN6%2Byh0A3iIGX3U7fONP%2FTq9NIGQwULR3gg08W%2FDyI6TRO3y80L%2FaaxbivI3oRtA9lEqoq%2FO63xXdHPcywHJaMJmrYEDpwK2XkV2NJyEZXzs89VuT5cEhKo%2BPC%2Bt9bkZuPr%2FoSSD0xDvE9hNTGp9%2F%2BgGcqN9olWbFXF8FK4hHVvNheV1pxV0HvXnWrxllKi4pZmI383rie%2FrW5K4pFKnConvNbuZBTiQaKQqxPsNwwwKC2xgY6pgEw7pr3SJxfW98OLMpfza2%2BofEoThDOp3JFPwqADd%2FOznoGnbp3RHg06l2u8KbymyLGU8CMSdqXEwz4xXekz3%2FyfherAnvWtWGxOt9PHFKa9MT57VKX7QRra6mjAsjCFTBBMQ0ccCZvrt%2FQP1Z0qMvoJuMR2FQaoPDOknenWpkxRrBf0buCt5BgdWYlcmX45osLRqng6iR2Fk4dQyuu8GMlsEfxfZ1s&X-Amz-Signature=550d597a4080ee701671291d2b2230cf80482013adaddcec858eb31d5b05f05f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

