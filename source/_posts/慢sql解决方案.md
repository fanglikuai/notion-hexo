---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN5ST6Y3%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCZqO%2FjfmeP2YCYtbLbEqDmFV7XW5PoS%2FTzsHrxarF4wwIhAKGXiaX%2BVQrOcLe%2FBa0pKfpxxka4kxpVEelF2I5id0mFKv8DCFsQABoMNjM3NDIzMTgzODA1IgwjG%2BlEbkdj8079Jxoq3AMBDBbj9Z9IwbMew%2BhSnVJFnws5Mii6lFGpPA155dWdXoZ7z4JTvYf9mQmAIpZbwldA2vJg%2FWgE%2FTD9lmbol71YqVha351%2BPz%2BFhP3CwxbigrfWtgoIkQ1z4qgWA11Psim%2BnUpBhdttSPVcd60Ts7kZiDKzaY2WdlM9XoGhRK8INCW14JQtifyhigWI0MzUrZ%2Bk1Px2GAAb4s6THVviLk0ZeDzyq%2FM%2F4ATagskzl6KuLi%2BZWTV%2F2KnvIbKHK9w%2BcGZouPv%2BFw9lH9k8hc88GMUFusIegSmuLmfEnQkxdliXhYpecjY9ok3UNGjf0I03%2FnMRjTbYdchRLTorWxvmxnBFfAIWnoVgXMyq0eGO41uHp3tdgsjhnwHFQm%2B36Of73HDyNBdNg0njGYKbVI3uCon1x8rkIsn7zeK%2Fca5y1Uys2mo%2FK0Igd%2FM6xPXPzUw0Z%2B1i%2FKoypxnZeIxsfbOTC5HH8I2ogQbr4D3NDjjpNTRkPtCZ1UfOe3rBXYYQRGoy7QjdifwksnUc%2BmdbXmJ8jOmN%2BQcBvhqrJI32ym2AQPXu5KjegaNyTdLooubHgeFkiaBLWvktLQdfXcUq8V1W5KRzPJv7%2FZ9tdSRQ58%2FWlG%2Fs2l8rYer55QXuR7uvqjCVlNrIBjqkARfMrphGr%2F%2BCeYy409vuR%2BNFx41p5jwL6vX1pFZ3sNMEYe6%2F%2FuHnoEXYeDuOwUAXcogaQ2fX1XwCaU8xrbzT2bZ6LvhNYhxXHXL%2Bt4KSGRFp4ZaWfxfEJbgcGjX9MSrMkUAj%2FkOSDGCZRlSuRpNFOuLCEUHCGuH46Y%2FQ8J3tzSpGyAyA2suqdYnsRAHwtAlCydv56AsFqgjQeEKsZoU%2B5gMfIrh9&X-Amz-Signature=f053175905a2a2e41f8609e7fed162dc2a7f16ad30ada2694279b6e8f26dd1b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

