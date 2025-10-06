---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J7LWPTM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBpCXT%2F1QXpQG6vq4OkmTsXDHLBDwTpo9kXvMxX97ThQAiBjFHADEqbx1vstGhBhum1ix4ziUevh%2Fzfl1dEIzWsTaCqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMc7LB83oQEr6UWQd5KtwD9Lwh7K%2FcNGKhoqn557I1%2ByaNAr4dV8578On0NTR4dq0pNrOMH1ZIFKRNVDaV42EwNb05xaxEAldJAZfmdN%2BxA%2Fgvm7ncZNDfxbzQavDW4z3%2F2Xa4MPaz9UMlTltflxcRELVDRAEcCewoabzHIA0yuZSZ1ePgZMmjUGv%2Fr9G6ZIkWRKfCPXdBlGgTc4ISlCJLGt1EcnYcAJYZ7rRLwl1CFkNdpgofYA8lbPkql2BMwQvNlRcLaob9gP1s6rHuDO9ySf4Ug9XsfdwsvQUkWWP1irhgDBjqzu3BomfhjLIAgsrk3Orz26HD5SrFToS1f3kAK4OGWxo%2F0PzCvCD2uQkYsJRSOJSvxy7Gf%2Bd8fR8zZ%2FfbNE5paBUfS5o0BiSY8ouKT6wlrsVogY2IhuRkyU2BeYGyjG9FsyDSHTCTZMn2mU%2FGAvhPmccOjDW98kHvlJzn%2BuCljHa7gxgjipdtO11unuQfuWaSeXxcUekGo2v4xVinU3EoVPtedtYz51VFqyXgVMfwiopGWMB0MzTO9grPwsAWtHQV3tV6ShF%2FLt29AY9eWlK3MkkvtOt2oCV1zAngSS%2F4ySsBUC%2BJLAGiJVNuD3%2FraPolYDgKagEGyi9TWXOC5y01OKHO5sx%2BD5gwi6uQxwY6pgG11t9QbaoTnPpDEBQbIPpZb4JZ8FnbcgnV8X0TzBtH7KAVDtEtJVnZdD2OWHYY%2FcyGVfVN3jOYvqYslmOXdkqed8EPgdo3ZER9%2FoFKlKmUFL2b8rEsNsNivpA8MgZnVHDaarTwsKPZ1FNqbD%2BeN43XogjoDeaYthGrAgc8%2FB%2FowQP%2Bp22JnMGiJHPUh9Mlp4%2FvYi3oVIjYNV8k4rjgeBdvel4tFfO%2B&X-Amz-Signature=5780a7ee325b85d20fa5d4b1c6ec5caac907a3bc5dc4f08b0ab5d7b75da0aec1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

