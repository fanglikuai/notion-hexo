---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWL3IZL%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbrtZNrmX%2BvfdTPdDRvRNP%2BvCblND1r5L7R%2FE7JoFlQAiEA25HiC2DPmx8PsIB0jbFNMHyijNglVjGbtYsvTyqMSl8qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLDIT5qgNbMnt3FbUSrcA0MtnnL%2FzHNz3bYgaZXRgFWBe5iGV%2FVzo19kMG0HXUWYmoaK8MAuIWNnZUzNHFi4CbfnuiaCmS14ytjYopuntFlY2O0%2BrauZMO4YWlLdYA9MmtQo%2BGQrepagDGQkfMQED4p%2FB54RJcEQqUPpvynD7425MznIM4Xsuacgj59y%2B8U6tkNOo3nmwI7fktoFsR3sRvoIlknTo7KFmEOPiMRExV8Mfe5GlJRoFqyUpM0zsqpGakObj2ARb%2ByeUo6nPMkTTkYHWkl5azi8O1a3WBlEuU0IeRk%2BaoD1cPbygMvRyNTxiB4SORB8n4P%2FnEerRwhwtCV%2B8bDOm4No78Zm6OXXegXDqevKcF1PI2DN3HsezdOUixg5qHY%2Fa8iuP6JhhjHENsax5AYD0vVG8f4OD2KlORtmpOA6C1xZg3q6iSj0Alytz%2F4CLObridyh%2BPY1apdIAsP0aGHwSSLpO1uZKTef%2F2C%2F6JugjhEhTIWwzlrlBDlV2q7e4hmJCpVsEaK%2FumkTo7L46JMENeC32nUa4AoQXFQHopzu3pJwqmirC7zkIJE9xeRNn1AdmwhzJHbdi8RPg7J6OlQtnfmDdBRqd94wGHSDo9bEMEFxZf3xUmR%2FNZ4B4G9Jd1YmQkZmqa1JMJWc58gGOqUB6tTTNgwGWUsnSDgoWZf3EfSAIj1T%2BEH8VNA9n2VvSJFo%2B2aUyptADRD6n6zvWwR7mQilsUb%2Fpu4d%2FyTm8xpAffkJAJ0SGZT3rJnhwozWugttxyfx%2BLhCAt3tRHOhSbp1b9X%2BYjbuInp6PnM4kn0R0ebPATqmYds1N2hEh2jwTeW%2FdqmFYGPR0cOW6%2B5roYg82WKqi0kNHZn0QjZMQsk467laI5WA&X-Amz-Signature=7b5ac0a62fdaa575aad66f30bc383ae3f4cd44f2050e1a471d4808d30a4d874c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

