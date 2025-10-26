---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S354QMPC%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T060059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDpPpGKEb4hcT1LMn%2Bz01rXuFcK7b1ib8aDVSJP1VrlFQIhAMpakNBm1VGiqBgH%2Fu2Z8bMwwf8MUjSxgdo%2FtJ2rseluKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyfl89CLXD8rvSAHQgq3AMBG76FF6Qu%2BJ09E9xK%2Bzv1dh2XDjTwnM55PBe3yLFiTiodQe2ekdrHL2cHSR196oHb4R3bqNeUN5zgdUe1LmD7%2BKw8j5wf31JU4X5CzzeDIeLUpsig9szH3EVgOWzZGhNkG2yd6%2FwQWcbjNMYGGTIRs9WBawq4fzqg96H8KxWBgY7LKK%2Fq4rY49o1kERMwNjaNIZCqaaejei%2FLaPf49OrDc8kyivGaiGfBYp%2B1KCph95o38eN3QtSK9FN9KaHblPOazPlOehMYgV96%2BNjHzxRp6MweQHeAGEle8KfJeqdCQUabLNLwuTpewagRwZaIBMy%2BjK%2BA9vp%2BBre7nkSRrORJLlZn2y6%2B1a7Qt18F2tWppqp5zboF5Vkooz0v2EgxY%2FabmdJS9RdCqpx8oubup1HhXooQvQflVAW1tP8vSVWPangLAPiN4rhasqyL4XmQvokPpEpTZTtCbUMdT6vC2P9cjM2xZ4AivvrSmY1nJRBooOOunwrWi3pxV0Grk4hIhO17GLRXtGtx3mE7%2Fk%2FctgeNirK0sODtDQxaDkz%2Fee9S3dpntPH37ViRw3IQo15b1fME%2BCzdvylMmyLiBWuA22XVgueHlD7PO2kxaVjfScbwHDCx1THGsO7tvfDjeDDX7%2FXHBjqkAYKbzm5c0g3BHkFz%2BzqagGFvn5VuJUcgqLMgeGSJbvkv2rnQNgKZXzRORlox%2F7GKxw2dSdjBTtP1WrDerMilUk2Zw3akYHj6p7AmCxdaLFWgUml2b6YeYX2w9PAyqWsQhZx%2BZJarqpwp5lnYulB6zPCsy%2Fk6lDHx9AFimR7S8MPY4o3tVoVz2EtOdiyemNuwg0gsLWLLckdUFT7gXX2yFNQOwPDA&X-Amz-Signature=8bb9ec8bc16d080e7a525afcf3b1092a632d4a252db24675c85067cdec0304e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

