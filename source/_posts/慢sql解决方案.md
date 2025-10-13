---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIMJ5EAX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPtAdSWAGO1RF9jBf3J8L2sJkbnNXJYfx3mWx1UWdxQQIhAJlBTlTIBH2XzGyOD4piP9pDvWmAWIP0JrC3Y7eB32w6Kv8DCDgQABoMNjM3NDIzMTgzODA1IgwbHAV9y0YGenWKM2cq3APxoUrWwFTGyy2g3%2F8947pajp3b9Sn91tTou85neM5tRe7m2VqgcURTw8nGk1a8J2HgN4%2Bb5g5mu83QAPym2FVhnSHrehJnupiS5c33HBN3P55IK22L1hSMaioj%2FL148%2FJNX6myZB0AIHJI8b4malx4vU45xeGX4n%2FOrXNq4RAEmtiD3IfNu5hV19pA7PKIQ1iA%2FQru1e96TapUbBsWJ5JewwF5wyURj3c87SwhGmlJ6Yc7J8%2BzcSn%2FTDOGE1lSpsGFNVH9GwQL6WhcibOEuoGIilUGxCfL3weuORq0Af%2BSiHW1ZguUYV21CjpgPFySlzjHhfDDpsWzm%2Be0LUVDHgMc2lkR8xJr%2FC2tGuxpSoSEfzbV7SR7aymrKpHgIB%2FzqHk%2FjlyxTNbdZ63Bv5A1ufj96STJGXHKdgE3O4Yr07sy92P%2FyEhfNnoXbvgXrPLLUwKla2pPt%2BpG3hWIT2i97l2N%2Fm%2FScsaNNO7YDiGqpDe%2Fecs5%2BjnJLQSicoS99SQxQdJGfXuu70U8C%2BKsqdTA%2FvVbr610ualwml%2Bj6SL%2BJSTKoW7ocP3V6F5jE8uLEYsw4bzUDabD5Kl96%2FMAcmxHEQIDD9fxTzXNalp5AMHBpkCfOdQSc0YxavoktLiEyDCC6rDHBjqkAUyDsKW2C%2B0YMMwaOlwJEuxQI3QVkU47KwyO0kkoJ7FHybAr4urZjN1wSky3WZdzFBXpUtToHhk8WgeDqEUQLmrAV%2Fm7riSbUzTy%2BIr27JS9xZcqq5u8fAepalR0EcpDVI9ccdksFf8TOdZ1%2BINC4zL7fZ3H7n5OokcZDiDWuzYSjzdV9HpggAiVLu9sEfGGjX12PO%2Bb9Te82A875awuSbtRvF1P&X-Amz-Signature=9db0aae4db67d3c2368b5cea13d598d9a1c02088a444914df53fbe7a90d53667&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

