---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQX46V5D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0K%2BLMbkLqnEdUUMIhHGtFfW4iRVnO07dYrFg7vWFGOwIhAM5bQ0iFmeJcLeHp%2FDbluR3IqazDc6qd6xx2z6okwwgHKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyXPYwuhKmoeHTc0Ykq3AMGzo4KVUACrDyTQk9n5DmDEQqDlYAGiassKo6tmIPuiQsI7FCbgLeZePDlyaYz%2BllFnjFsxt3R4lGtPlGIT7LacUxoFCnuhEaipmRp8G%2BJbBoW4Fn0rbtrK0cDWT7zDAYqRWJrjWTfJJPe7UObmPe3UJpvG5dNlPQycA2%2BN1%2Flv%2F65DirxggJloOpCZTu3FV8wTKzFRXs%2FY2OvTfjDacxvdKkVTV5vTIRUBTjdqe2ISnuU7EyT1ixprHoSe6AO7JLQ4hLc3WOlYK7TrEaALeP9tdfYZAtBfV6ru%2B5KqQK4MhzG6Yd9COEJ0YHPoUui85tqiZvz9bfNBpaVFOPJRuBy6OpTrVsp2Gt%2FpayN%2BDiQK6sQx6z%2FmNWphVjUuVwUlQL3QBTTgp3UnVXTxAa8Q%2BJhD87x6hA1P3CX17xhxmtppJfooF3%2Ba1xj8ZjjtGeI38t3liVw0%2B%2FY2eFc1tGSA7XGqyhLufIawjYZqvdJvaN5a9e0zz8Mrin6HcLkvyZ9TsyGDtvTnRE2IWHTpGk3EgF74eStymxN6aDxdgvsthz3bIvozhu%2BFkLhtMRkPBT6wKQ8jLBVatBxfWajCGsf8pG0VwIXuqTiVVuYVLjJrGZ95V777hPzlm6xKwbw3jDi5LLIBjqkAb77p3%2BpqiQ6CLac7ZabAV5vVtplOUUw6XjOQmszdyqnkIHr5C3o8SbIea1EXtQK0M%2BjIH9NmWQxo4iJ3vMfNTmkUTgu24rsYgz5j37aSw575HFWQDXfoFhkP%2FL5maKXZvicEDCa1Kk9c2ZNUqB7Ka2tGVTW5xWHctN%2FsvBzTqrR7wEaDO6WHa2Z9O3Pgs3FrewHIHlGa67z4JwGDp2EJh8A1hBR&X-Amz-Signature=c1c9bec0c6b72c9e7de614749f76e58dd13b87fb622092c3ad56b6b514c01966&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

