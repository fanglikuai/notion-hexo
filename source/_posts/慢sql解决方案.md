---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YEFRI5X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGbmmsLvEzvsKsviuV12PYqTZ33CcOoAHoZn%2BYFyTFxfAiBmBoYFyEYDdEe%2BqVzDqtzbgoVP4WZRvVC13Xske3xBgyqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTryColROW7A9TPT9KtwD6VLRFD92nJa8H%2F8zkfL%2Fu6j3k03Ej4zLYCP1T9J4U5ia3ACryVOqeRHZms8p32Kc8otjWJQ63H4K6sItDX7PSXRx5DAUHXnhr9uE%2BUyq6tr7q3gmMGeJaXuMJxdgUZKrs7s7M%2FS2j6hSRx2hkliuUQWYAhK%2BriHjPsmBP64S72SBQdtU4iWugNFPW9q6XgCJ2Wt7MvXixJCJ6SVoqyWqqlzw%2FHKkwPDCxVIkmwY%2F7K6CxLwnDdHG3PQ5jcZzK61riBJ3JgpMu9ZvtCw145rIBE0GGYp1iS1X9QsJ%2Fu%2B0ZPbvkSRXgeetzjVwbcF6LPLB8%2FWJeyp%2ByhDjvqsh7N%2B9xu8ugf%2B3375W4vbJyEOOWemBQeuNajFVQvaawkY0UoY5z7LZq6FnIlHV4Kc%2BwbmXwD%2FFKbfNCDK8k2ush1Hy4zX9lb4yWN%2F%2BsCAgwgBLhxkEb%2BQKsMqPsGO0RB5%2F%2BUnGYdTyo6YIxwKp7HdrVUisD8SQTK2iVLB1qrr3Pb1msVeyOFP5OUNGOMxIqDBrrfINvI6lq4e7brdwuRyE2oB2T%2BzEJP85lCm4o4qrkSFOZ7YPU6mycRJtd9dHyg%2Bws7PjAvLomSMosxoDJmPmce7Zg7ZRSuxH0T4YyS3mr3Ywmvn6xwY6pgHocZPCEarwd3hVPHdLxoLk7ufRj7cVOAI2iqHNkW947rgfqqmI%2BxrlhlhmEdT1JfMe6ox8B2HJIQtnVk2OcgOKTTLKqzHhLFELhGHRp8jlUIug%2BgAe1k%2BtMVAcmDoKSUw5zgKy1KrNzLKioyH8Hc9MeZjGI4Pivgd8Hf77ZSPSc4HeN2FP5xKC7PjQ6Hp1P6sLDF6nl5vFdpceCpl9lXYXE7p5me1q&X-Amz-Signature=c2f4ab8b1da426c4bab75219647acb06dae496358450621a561866bf19f497b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

