---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRAIOWHM%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDPzCd9FS%2FQaY9UfKCsS%2FqNHOYEEOCPYds8VWavfYJvDQIgKhIgmtHx8rvFsGauqBvBzJPPOPqRFvFQBsJ1CTxWOKEq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDGnWQTEBLwgZjgKi7SrcA34M%2FYpvQqDQHtuLNfBcboO6EhFNgZe%2F9xm2tTsoNG1521gtGVxBhwfN3Sbf%2FnrPNuuOzh%2Bs9CCZDqGSCYtomAhdor9LPMJxSZorU7GWkQMZFhkbn9FpMjdpnNycKeaD%2FPnSnHMVhRXWtUPcvP4RKVdcPbmtHlc3mfxDKJ9pofrUaIEBk8xuKJuAVJ9LspDEnhBVEipiMHWGg78E7CI7cVuaukGs3V9qmx7mptLCscy15iR4bEZ609icBEmt4JS0SKJMRNhMnEgu9RhbXsrcO9ysLi24olwueO%2FnJRKVbX8e5kuoBFpGAXLa8PyQn%2BlZobetX0aJ0CXdej9bFlyff0TLNAdJvpfcacmSjGGqRnTK7fyOdljaOFaZrLR%2FUH5%2F8aVAcbJTi8%2B3RkCdjKQoyNlzPkMLp17K3zt0JFoH9EmzPiSCYJ7OMkcfCvmMOrCSlPbDZGil3cngRaEIwtDP3gOh5jXVbMAM%2FqkDyFQQo0Rj1vyT%2B4L66YSB%2FgL6%2Be2e0V%2BItfBGH8MhY77MGIj8iCJ3HrP87BnmNJIRDSTuJbU2oY6BKZrtxWudK3BLiOvfjG%2FCtYMZ%2Fjy2EfwFiOyrqo8lGNN58POw2gJ4OI9YO0Cc1GUf2L7j16lkoFFnMN2s7McGOqUBHa8GVCJa5172mouCRp8KcDS%2BB6eeWOBrwSwbuWb%2B2BGGPUwnco%2FmGHOevWCiDiqTltIOiUnp9DqQ6rf1crrH%2FCZt6MOBonSMcLH16mv6FD1vgt83Inzc%2Fvbu%2FBrKIzubI0%2F9Xa2%2F9MgN4owxmiNaYZWhLTjbFscxasDmMB4vVYUhze2rC%2BocmTC6VNKnyntHKadWblJFNkVu7p8BQFz0ApO%2B1nem&X-Amz-Signature=cc3b85cb08e6aec623ecaa644b78b6495e87d57b6fc8517e52587aee1d1ae245&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

