---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675OY76NH%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFALxfYn59Q4%2FiZxlKVhrUpT89WjdR98Rw5xnsCFD3QAIhAIDyf13O%2Fd60CBG%2Fnkor95dNnkrzlyWC6Xc%2FlBd7QSKDKv8DCFcQABoMNjM3NDIzMTgzODA1Igw5YSsT71wgwo%2BfP3Qq3AMG40q5e5sH5h8neQ9CHP00Bcu8zS8b5VZ6ieIogOF17FcynxWiG6GPUIAlniEbRbk1yI54T7oP%2BiY8GFoJ%2FSU7XbFOA1q9kwO5XuHz2bu5%2BxwfN2kanxzI91CCfP55g1UqclpSN0NWNX10jwgyfkOxGxZfRPDuVrVqK0dIb1bKi%2BrIm49gsw11imS8o0OdptdtfatKeduyboOUDn%2FDi66xmJ6H2XLuVhwsQkwSFaBkvh578gO3WeUbC2d78rASVow8xd8hwVMire9YpZMt%2FBzH2C4UZ6WOMfwM%2BF0Aq2IlgV3Ou7irnJBLE2C5z3e%2FyTXxAr6VrBw27QkSLsbo0aUr9GNbIt%2BnvxrNASetTUNMeJWwenHyPRLQg29IYLfAeds%2BGr%2Bt19eiGIFZDeVXC7ItwWF7DtssME0PYOtn6bOinPApkiW7k95oFU19PiSmzMkMfU%2FOzVkwPNB8%2BCYx0BrfShQRl%2BdP6GAlXN1%2FnevU0pJ6VqYbci66yi9KM9Tnw4yMGuJdyC5jwiLBfaECj7RP17Ym4lL%2FYEn1ihM78xUX79Yknj5YLLt%2BFAezixdJY3TaGyssKh62WArLrf0GG%2BpOEHNJlC8ybohQJ5ES4OQyoFrasBbNwhA%2B7e%2BQEjCYjKHIBjqkAQrulMchr%2FTsB%2BQdGez39oen5lRZYUeQRNcruJaFG6E71IP5YsxeKsz4si8pcjT66Q5Mu3tyYxengiRiYSsmvCJphVVJ3S0W02bfmzOXDnpGR4OgdzpE3qcMOGzMqQavGx0pfnbrSHlPJq5BWLe30HCXaohf4uPqnRcaT%2FyB%2F2w1M5o6FV8sXFHuu3mFIFPLWWDvcwDh9Hw6W6qa59NvOD17jDZp&X-Amz-Signature=847d8c5be783215ef5a1a2e9b832bc51542fc130869727567dfd97029410d6b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

