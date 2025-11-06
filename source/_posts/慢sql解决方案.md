---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZL5FL477%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDHAo1GvhdhbkL3mwgMrBl9V2UsqWct%2BwnMsZTGTdPiBAiAXHN%2BXzRQRSgvptoQhPJJWZx9vhToYWXtaob7pKyYuKyqIBAis%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhUKWIPAI3jRVOa68KtwDJDAsRopVAwFr7b36K4ewU0CJK2k87CQ7zPpvp%2FUR3%2FwriqERlQM4N9NLXoq7zpzm0j9uLZ9v8lOapBn6%2BZvY%2FnPwx7cfxBHqYWQAds7P6SulBB1337iuwMVMw4Zl%2FB%2FwdqXR6%2F7qht%2BO2XBKku8LIc0vJrkIMgY%2F56GjDJ1lt3F4HKKZ6f2vP3g9b98Ly11Klfmg1%2FbcIjOa0BCvtcarlkSkDQ3C1F0YygC%2FUv0t8pPTS%2FYHd2gf%2FyrxoVpV9QxxYD1IeKO5APKg20w46Reo7z62A7VXKkx2o4oS6XbqyTq6sUPa5gAB%2BXGZlZy%2B1paTVipPAvON90d2VU%2BGiCBT%2F6J13gykI%2FdrgpwGO5CAdIuU5Y3qjti7PjuEEcAQjXplrtNC7smWm3dfwv6M1tKngbfwWtHefmiR0mwzReh3k3hkPuGsWbukEfCMMrkuoaVfPR9D6NWL7iegpozcilG3It6RLMMWn7zWN4I8q10jSNwHjoPKq0pVxUgqkVAZBErTKnSTrYIerCM%2FdXgeJbiR1PK%2B%2Fx6VxEo91TGVLFrfHY%2BV3DIU9tFv2%2BGrIf0vtwNIbo1CHSuqw4rqkfnvXnfr%2FkfYzjIXmFT4P1Q6hRf9wcp4HhUZaFjFCzJ%2BuUAwuuSzyAY6pgGH4aeaUWXlcOfp2S0T4XDQ1%2FJKBs%2BbOPe6naiRflzdKMu9DHbYo2wvC7ePXNK5RSQz1YVsCQIaelAWNT0wuQYfTnfikBN3dUpcYVzLryzE2O5IVULlR0aegKeuQGlk8RMpVbESAQ31u6pN0Q9TM7AqZZWQJacmyLzFlgWprxoZWVUfBtSZIym1EXWZexdSygWaqpuUN8x5Yw8tSmdkWxegqO4KX3Fq&X-Amz-Signature=337ab6cc89be6b7846518f06db633879d26ba70a89d10985bc1cc289d8695bb2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

