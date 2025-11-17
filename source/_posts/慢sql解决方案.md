---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVWUXG2%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBDy0jOf3nY7u2ZG%2Bs8XUxMPFpMD8L5QQauYBo5lUW8bAiBhzL2yRvVou%2FGRB0e5RgFUNMlJONKjgVkKmfGfbYcB9iqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM4IyensJO%2FVkN%2FeWwKtwDIAmMe%2BAUEYToJAOtljAnAFCSCWyBqes3kHjZsmzCi0HvVrvntKZg7iQn7pIi0m4y4L0mEW2NNiUfYBdIYsjAxpKTYzF5VN6ncg3tMZDfSqofN0IB%2BpZHnTNBmhqABJEAH90RqPk8OEq6aLg7IgOl2aKgM2k%2FdQZxwBtn1j8SU88or82U7maYxdkZ%2BoAjXJrmZwPsZDoTDA5v0%2BUZc6IeK8OxbE3pLN8z7Xd7kiHckkkZHH%2FnT5LUgrZhyXP%2FeWYwQjWOFcRSX3t5lgzTENYBjW%2FKLwl%2BzpTr%2FREuPvIOyIdtuGantGOUr68OmuYiaeVmpTeBo8%2FuQwRfgxoK6PwXvOz8LSzWq4YR6KwUBZ3jz6arqbCD08g6NboFMAHNSZnVnoNrqtVYNxgYm8JC98cPQ5OcSQz94CL5d3yOm032cggQeC34JSbB0thVJQrUoFeITWAl%2B4Adgg98sFOXJpg8qSF4X05VQ3gzF3t0IRXqd53gzu89VaIL1bMTTqU3%2F1M2pQOVSAuIIECkiFk6zw9qSdZzR9OWurEdlBcTQIhAJ8wtS%2B7eRMcrigJz4vb7gPfVLcB1g1ROcmZbtzEGeBufOCoX4XsXyTYipwM%2FziZFt0u8chXw9KuDdbUep5Ywv7XryAY6pgG8Q7eedQ5gFPgV0OC2fHxuCilcfuPzUJyN%2Bix2TQGYaq6uExROLLR4XJVaa9gJNIEc6xN2W1qjwsNr00GXH%2BrxacKfVanmf%2FsjQMnxORWQ1sB1grb32ZkgJswhAxiWze6JPoDkSxc%2B8jTqw8gwuktvwp4YjdUGhZFaKvOn2eEQqWNvwtyHgf2jteNqKbter%2BUC0qLDOoT35XqUVb5%2B5ow0udyG3g5M&X-Amz-Signature=182102a18913c893170d7a391692390e46030a2d22fe01c136b232b20d638adf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

