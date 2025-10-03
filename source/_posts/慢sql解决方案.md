---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGZWINKJ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFyLwsgm4GfkEyfsJ8UYpmp1EL8KaqgrzwD82znUIsVvAiEAxL92iFaCe6%2FXVJ7mIWgaq4VPgvPeJJHXoGUye6SkEjQq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDJyW9wJtht50pzWMBSrcA1G9FRG7dMPcROEFl02NU09tNuWa5rj7Bwkf5Ke5QRAiY83Z7gccg4XJkF0aFefA341CH0kHzxv%2FKgK8F%2BJ%2BrdqC%2BGnlIphd2uLxAgxcVj4kAO9ll1ZBR3aiC3ipTsamAbHo7yHEt4wWM0tQS7QwdlBif98RNYHGMzGQDy6aKWCqqbFpAo2EOZOrDl2A%2F5luc1uwuxII3LJMFj6PF8DZt4AKwX3sInUQMy2vXeqIIsoy9jhuV07f5nMyN3jpsVWa3L%2FkR7vQCwVdhf1fQkTbWIaXvwk8toF0DFhXlCT%2BFdtQTnSDF7mrZ%2BD0KQt2n6qnXWmbbkYNKc3S8rPEjGcqYHUDOEFo9S2AaojbepEcPag8AfcjamCxkXiT8mIDSVCyX1Hwq56atDvCAituDfs%2BKPuh5hHA8qezsq8ZMmZzBtX0HpyxgaPK0EvM1P4cBbEVHo%2Fmybal3Av25DCnYFhx3YVZUN6GKo8DofIyUWqj0cy7wD4ckuPWnY%2BccmIMFl6uf6nmdr2%2FRmMkKcEuzSjXACEmaIUh6Z27V3VAox%2FidMYpWX6yL8XMR%2FVEeR%2Fp8D3NEZ1gL0xzacqGtzecv%2FUfI3%2B6vLoFZdKkNT8bm8Ht96wmjDZRnQPJ6%2FjUFI9GMPn1%2F8YGOqUBnMrKEQF%2B%2F83H89WFliHgjDX%2B105xSWgeOblHTZear8B3x%2BcRXd8A%2BOFad9AaoclhKG5nEvKx4svcYTfcxgdzLoDkK6qkvRzyqhpb%2F1oKqXlEUMwgkZmRbnZ8wPpsQnugVSLLhcSX7vXBN%2F%2FTZqQosEqfbIvuBxv1Uvv2d1KUZVxWXZSIsZkEJkSQtjnHwm8IYS3QEcgpJtSxcxwrM%2BdH9jONjOni&X-Amz-Signature=085f64965f2efd29b0732489cdfb81c4740ef712324f8425929e640bc9db9bd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

