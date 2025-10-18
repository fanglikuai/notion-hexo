---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNILZVK%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIG%2F%2FXWflu11w6A8gwyPfY%2FK0sFVShBKLqvCX0B1cObm4AiAFckh%2FYi1o6agtAkAoYd2kGyZFgn4tMxV%2BWwJqMwsMXyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeNt17MILK5JAlKtdKtwDI%2BlaXAphEcKFlTG%2BJACitQqJ9rjbOic8HPg8j%2BiNAxGKwFHZ0RpoZDKUjj0yllX8VpfQ6ggclbNT8il3y8BAb%2FHbut8TR5iWZa%2F4GDvC%2FQ9lw0P%2BJLET0pPeKFCYcA5fDDjB6wzykdqmkZPY3Ae4M%2F3mpRyVMSZ5yvQcd4DiDMZtz1NGhlgSe0zUiyPU9TUhbW1X%2BDq8jUJrAOEiI8u5kVrWIFQQ6oFQv4ruB5tkmfauhN6K7Q16k81JfazEsdcXmI813ahzqXXLVXGsIrByypzgkOcUsXkxLa6O0uIuXzw8Onou93UtVkgS0lrjHVeRLiKDixTuQPlU7HboeK5SgMe%2FURNUZp3bU0uD7POV0Hs9xJhZJVfEY53gGOG2G0aEEEWNcG592qX6n%2BWzsFNp2%2B5uzQ2nyjk1eM%2FeNMg0FLYEg7CQgumBiaRvCmGlrdDgxUhw7OcPeGHc5tlNoEp1mfXmFuvQvuIt5jBY%2B85N4uYckBXiIVKDyNA2%2F%2BP2eecZtCajKuq0HTGgLaNBnXUAukvf3KdKalKHJevqJfbE7X2sOENaC3Gfd8ouZ7e7Ajt8AP%2FHgxDXE%2BeJ6hmL9hw3UgRVFnHHRaD3FsP2xgF6PObewbnzRJoKg6yBStMwkZvLxwY6pgEAmZLpLj3V4hVNsDSuxnTE3JB6q8s6hL5hPMRB1lnk1ELEdIzFSeijTrmgKsplYVlMA5W%2BRw%2FXNzYkVJXRTP5ICL3E5l%2B24zpoYZtq%2B4WQGgWhClbrTeQDNl76xBfWQknI0gvUdhJrgbySuEQV32Ik5ASYSZHqUEDdVf5DI2ZyeHeuON%2BPU4F5w0gSP75KGYz4f9iEUK%2BxjHDXgnOztGCHWPjtal1D&X-Amz-Signature=3707648be2f6c5cd22943be06317239b34146d6569988f1899c93840233435b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

