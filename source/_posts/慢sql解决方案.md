---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBDADZVV%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE2PVEvxDPlpMMLc47NKjlYVpHMaiZeAexLsrht3rykJAiBB%2FtrDxnO6LhSFE97UClTSHEqgAdZj3MP%2BpFL5L6T6qCqIBAjC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEE1nBQE7hx8mfPyqKtwDHxPCnEGUt1XbH%2FVxwiG7CWQepxDYqWw7VZZzvvEDkv80xL7m2U5PJRzV%2F09DrsPjZidJUWLRo00SAuRv8HFIgfZu%2FZtGeIofIwMtuK5AtA5ugVn%2BvQ7BK60O8VS%2FN4JYN8jfQTQIojuI0CazHlivdhGj2umBcuKhO%2FwkU0kIqSPN7CnanVU8qp6SXT0UUEjX4Ckv7EeScwktRxu4AWFKf6xWvvHXgjsv7A1Rz9mrEQ%2BqG0GMTY7fP783jNq45aafqy%2FrXIQRtAVyqVHLhIbwD3L5WsBJM9xWtOYqMj%2Foa%2BFK0kL4Y%2BESc6XI08E9QF5bMWZpwyOChayuQz3IuxUgUYUJMJCgKYWEEItUal7LJfc6a95U%2BGtuTuW1ApUl4F3pclDeELxHskLqw%2BafcSUYmp10omzRCcPkGBPLZt26noTCuxKnghgF1u%2FrDNK5p%2BcH1lczaTcTUtTprurnOlYr2%2BUafhD%2BHnS5fFhidjdt%2FCX%2BA%2FxSw7X5Hs0wlPsp%2Fgd14l%2B9VeRR%2Ba0BLCHMTa6ug4u%2BOF2XiU9Gigvxr%2FRvAck7srASoSmrb7u1hDEb2nOlT250iP4V1qXZXedLPDk5ECfpQeQeHrwK2CUIUqcVOuImm24WuDwfU1NUNhYw0eXwyAY6pgGEoaV8aWs0NQzp8uwDYZWpXuD%2Ba%2BzRKoW8LoCLcjUeIoNLwrctOq%2B1ZEoqePnEn8gnENWaFEsdMQ1AE1OUeXTJxI6G%2FEqgRU9QavpXDRMM%2FPr%2Bi7CJPs1W6hxuzzj37r7acWU8eZWKZ8xHJcQaIcuV2ozg%2Fz54eei4CJBM9KU2BVmjeHzGmLl%2FSHx4%2Ff5yTgZ5iSVV95el%2BOCQgR7C6eQluXRyV%2Fbo&X-Amz-Signature=f59003a6c45f2da182c6cd4a8a8952a6b56aa84db97a8ba4ae29eff4466bfef3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

