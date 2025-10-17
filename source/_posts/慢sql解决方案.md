---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QCQOOTKV%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC%2BQL3vvAtjlAQDdWEVTxfFKJ%2BeZ3%2FD2s1s4vA7I9CKKAiBttWJ2PPNd%2B3hMYMWKeH5NCnV88FdDhyDOcbDCPXbkPyqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjnATRKk94dUgn%2FRyKtwD%2F91HQK9JDIPOnTDAyuVRfl%2B%2BRhG0TFz3L%2FlaJfLEfSIuuz4ny6VFQnujCmehQkn2Evej%2FU7SxVf4yK1J4HPE7ZQ9e9%2B21zqS6ULIeW6hEA4CBpMUtxLZvG%2BrH6Zb7N5FzPqW8kwtwQsfGIfohi8YnRWddMiICZleTNl6V9eUt50AW7cC8MhxhajlwRLk%2B%2BgeX17FtbX1CVk3zvA%2BHwVMjNLFuJe%2FsopS03LEJt7t5SBUorbNI3SVeV7D99Zg9hagruZpbeu4cM7Psk8tm8I676L4ahli4o2kojJwQSShp%2Fv3o2%2FHrsA9JMKifB2eGg5EitEnHpgXFrZa1JEiC7Fevpv16raQS2%2FGcVwYX2pyxeZRlFuDBzixHgG1L166uO8K1Z3YINvSAWCXREOBOkekVtbBAE16he1lDvz0kZ%2BE47gt5OVy0QLHI58nQQy5dc9npbZCGM%2FPasLkmuMz8N46nChPnZKViNS6qBVMNwaufwv7n5CT%2B1bYX9lRN4uDbhwjayn4ytLzKtY5sW9hLBvAMe%2Blo5dO0RvGdyS3P60mzWPSXh4ZLp4rhRY7iLCOHmx%2FAGmkM3qTTg165lTIG1fM8uZKqpg%2BsNDscxIuQtHrf1aF8EHVoNiCXaO%2Bm%2FQwgv3IxwY6pgHXas08hAUq2zorG9AcXiPoSeYc3L6Ys1uPOQroFDxLi%2F0eSkhDKKCSTCAKuh3pnzVanlyH%2FRKfu6GvwgghOS8iCEEU%2BJHUi9b%2BfTNiBectboM5PBPc4HLCysot7rLr93i36Jol4gwe%2Bd%2Bd9WYfz14Flrd%2FpMJZbubLb4%2BFvjmAKLrDIFm9J%2B0%2F6TiQ6hnEioy8TDUun3g42XSk6CE8NlUVm7Tu1YWZ&X-Amz-Signature=845c56d6109a09891300a2cd5ec4244f14484ad841f24fe78c3d13d6557a3b27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

