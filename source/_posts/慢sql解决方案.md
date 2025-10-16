---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZZPTKGLK%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG7AYbbNFKU5foKj82GNW7TovTKUoK2dMnBh68479EFEAiBMF9jwZbfOG2Ohdljn2v89UcKUltg67BvjSrsHcp%2FJFiqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMolD%2BKdyAp7NTb4TEKtwDhpjRdWimelQyzP0mBVt6JekfFmILPVCYin9Hfb1oYKsH5xFvQSUiRsqOAKlPzHdoNaG%2BcnxxoGLhFU3FW6FI2ZDzGDGfhtLUx82vssswtbtpkvJc2xKKq6GBt36xa%2FD3oIe0w7Q%2BibpXaOYza%2FBGROc9CZEJd2kUEWStl5tadcbhLLpB0o6len%2BdZMbCkU3O47HQ%2BbqL%2BeAehV%2B22HyftJxdzJ%2F390pUTRDFeW40BjOX3k95Zzb8adk8LPgLb%2FI8IIC5Nvg4no2z%2FIGHPAGxKX%2B%2BA3XmFrgSmJE6F7%2F%2FpcWqptVtnvCZlyGifhbn1a9HgF3H%2BMMuJyd3TJ2TirUrtIg5n8U4x%2BgjEvtzKKBz2SfnXB6a9MJOIfvP6s8obOqKaSfBQ0xp1CGEPN2asVaEjH%2BNgHOcj3kPBgs3vl%2BQ7KlTQP%2BNT%2BuXVFxIoFblAsDpzFwUuhTsLjVPYUhDJ3G9ONipYOC2ZNimEhoRiwyYvlv%2BxoBO0sQVgb77gGIuR50BIW3Tu%2BgOJ3b6UUeXdhvR7lGNcJ9qdWJxrj4ah5EodYXEzXE13Yf8wY1g9MXETw%2Fwtp6%2BV%2FGxudrdbgi6qwF%2FMIsKurKytYFruA4%2FMEDLlUl2%2FmJ3yvT6y7zfWhEwm7jFxwY6pgGBzwTz%2BE5WUPsXRbWXrlLByCNTM8cqpvP1mFKbpSR077Or%2BSA%2B2Pck1Hx4aprwgNCBYHRI%2FEd%2Fq638UaJRdpKanFf31cystd%2FdABFw6w5aiHJIms7bkMVnpPy%2Bnox6eJ1VUGkEt8sp%2BagtUurt5BuRjsr%2F3ZC3PnfzC23LduEDadB6WChKvwrAZDTqpivlsTNpDeAp5%2BZsPT5RF%2FgzNoZTRqL2KI4a&X-Amz-Signature=c9a7ba3da695f6eaa7fa6b4c334082381e2ee5a05434e1f004649325d4c21e37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

