---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XJ2BW4C%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQDdYcgo3NmNGkDqG3mPol2hI8Uoc0Kb2sOVhVery09fkQIge1iOuRx7MLxHTzjhb7ricaVIBf4ybmdX%2BNEy8Cn%2BYP4qiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBJdormG3Zf8d80rCrcA7I4TJvQ%2ByQEqqcMXo35TXn6%2B%2B0WB0C6MbmROez5nhfoj9zZvBe5DrVrmEC%2FmULc2GJUxT0gdIwGFKSZa90WfL0cheTAzoI2Z31f8Q8R99hpytLmaq4psP6PxTV%2FuOqD%2FSX5sV7y%2F1%2F%2BvlSuBIAzbagEAJrvTaS%2BKuewZQfzJji%2BQMcSOdwsgv59JuGUKw2IKOPSwSENTfEjlYIjDL8lyCjpVpuSfWHeYlkqgFQOJhq3yQZSsYowWBkG%2FgF9UkZe8PGVvBVUZON2oN4prf0pdoDblZDzM%2FQdAeR6aCUqb4FL6tvUoeVu9M4YJjgbtBAOax0iKrAsQSvMHDE1FUvufcxMbmTKH%2BpONip01JEN%2B5W%2BwPxX74FWQhEuytHPf61eVnEaX4R3aOZDSYqKUFuL7o5Q2m0YWrCLNGVEWCLTNMO0caMhh1ul8YBHPAZ5o0z5rV%2BqCrZV83VfBM3BbCFHQkSPLTl8P%2FYnYt3o2wDERbvM8doZHgE6VqZ0pGigy4S0UO%2F1vIBpZwcnKCfwiDCYffSos1%2FUDvK1ajVEXYt6VOW6DFwndwoYh9efeJ7PAPJkg6NQR%2FjIFH44jr9gg4ZZKt3a3f%2FW5uKVl62ix70RWR6JAi6FQlrx5WBnQuknMPPy9cgGOqUB0jMhm6S3ZAVAR8U9bAYS%2FcIR9DsLdJ%2BZNMkCSwSRLz4%2Fh%2BnWAP9IHlSNgb98Cw1I5PWwG2ikAg%2BTobJsFoHY%2FB%2BJajx%2BNfTTUQoxZVKxsQ1I9qFyhalw1JS%2B16OeSRQ7lKiYrZzgU4%2FDKfDSSq84g%2BI1R2h5Wy2BRw2mRccuUXIkzA7doquWw5unWDKzhpZG58geX5ZVKBsKTJLITJKOaej5qlOB&X-Amz-Signature=5097c5caee033eb6f30faafd747dca02b211b06bdf99c5723aaf0e15334a90ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

