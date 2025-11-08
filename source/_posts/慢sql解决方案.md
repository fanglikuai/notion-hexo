---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S5A2QUIG%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIDg7yIcD0EvZH8iRKUNgaJZgvo6RNnyzMKo7Ae%2FHYjXiAiB7CL2AFgUCq4QD7VzXG34XibnFdDpmQ4K2bln%2FJSNGVCqIBAjR%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUt%2F4GYfv2YSOP7zEKtwD%2FjojGXMkIS5b7ao9eD9rG%2BcXAZR0MmloO%2F%2Fxz3vr8Q7tmjSDYEocWpSzpJDaeLrzIaqpuVwGNPG43sR4lVgyynWBAs3qgSlhMmMJktgBKCCe5JcHOQepHdmvUMS5GZLgfYPD1w9OH9N1JoqrzXhD1rykZfd60Ibvz75PD839i8m3R%2BH2E%2Fqd2zOcvrINCpPl4FVfb1u%2FgUnIULiqlOTP3RZa8ZYKPxoqwTE6IbMZUSGPumdHjlv2wicj99UHfPzWZ63TouDJLuRDzH9Oa0PvaCJc03H2wELuLCsTpPM3eewZq1n61mrm4PWn4WyUpKO6wzFgHvT1wDDf%2BmXsOv%2B1YW8bT5nceVzKoP1EBa5phb1RYH5HRgq6i%2BpO5jCpsku7Vke4E31Ke0OsfD9McF7yxYbVMcCwGiPQMi%2FUZ1gsIqHSDS1g7ob9aVbeJV73GmqtnZHqs7v5%2B0cs5AFr0VYdNSmjapwXNwWldEpT3wUpJnpvnhA3skXIRS0EddUo7FWXRkNYJSqgEAtDWH%2F4qAeEx0HgmaPNyCOpqB6iONiSGokMiu%2Frg9aD2Xrp%2FDG5x8%2FuaRiHTNwZn%2FMP1oSr9%2BLNuvQRSY7Akw1Q0bzGLRmAEAOuCwtEpyBr3bucjmIw7%2B67yAY6pgGo%2BA%2BC2CntOCdxw5TdC9UWJu1pAKeN6w0tAf%2Fgx%2FsUGD7B57pms8R%2BlJFzGdATvJzj58siIKi%2BJWe%2FfL9%2FGdvqBJeboD9Q%2BpMWvaERqarFmU%2FXfDW2OJVp%2FTUIa4Xk8QW4ezghLptbYH%2Fh3rKP0tiBvKRwhZ%2BaZNVWGfXP8M0m48Wf1Fp9sStZAXZLjFU%2FNesx2107MFvFCQ3OZYfz0wTjJGDHROwE&X-Amz-Signature=a9094d5eb234ecf6e0b9c39d6e5365335b826ee2078b6f7fd9df391dde212472&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

