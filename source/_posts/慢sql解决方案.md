---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTWXVNZL%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T130047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDGYpnGst8h5PexKKnFCVdFca4ijajzcJ6if9K0rNistQIhALQoZmgJ74S1ooiI6risX%2BOhBNhtIvtZ1BlcBvXIrm7qKv8DCFYQABoMNjM3NDIzMTgzODA1Igwl9YfwvGVK%2BRLs1Hwq3APnFakDmKmfmxjntL4GXrktHzb9y%2BXa4A1Dy3PIrquxT4X5RUXoELv51K9w8SX0xAPccHXB6kIn%2BTfzIf%2Bhvauq58cIbTmZrWZLeVrqAjDOWXREd5Kb2kZgWEuPvdoO4p2lKBqPPrKuJWSQnxx5OWxVKfOUh42lZv6bcGkelQQSCfZCAtQjfjdbz%2B%2BtNZ%2FVKQLUJsJlaGZshAP7doJ%2BwRTNIu5cZ%2BlWvV28tn2c%2BYE2L5U7gVHqxSijY3pmiUUL1nld0RdFZQpES5a%2BqLaducB6s2SWOYSCZ%2F7fCvwi3vjK98O8JtXDECIzEy0EmRyA0d5MPz%2FcrdAjhQwSh8TQX%2Fd8wC1BzZhbD9%2Fok5YW44S%2ByuaN%2BtwITQ9D8%2B6c8GRGCcsJ0XcvMM0S8qPJWcSrf6e0OOAC%2Fw06ped%2BiX5cog0KRg9ESlrTIxrsb5tNYD5yGFx6AeK%2BBxE%2BhswwLiHCxnvM8FdVjZMtouivell0qmdqozg5QtjQ9%2B5LNH5koNWnyCpeKxkDbhM1cDHs5vaSGPPJ3XGDgVaMCdvT5Q730IlXAYGhKTVS1cJwEWrzh5knSXOtgCiynJyDt7iiNNOGvpyd%2FXkZY8xkqEGQTLNZaKKkhiqo5xBHg2u%2F8r2TLzDWopHJBjqkAUTmN5om0OT8C2rpnu6OlQ%2B5vKL%2BFuy72kU4y8ys3kAE8jgQMbbrH9uecYrnki%2FdAhDQoWh279op5FAOYjK0Zg%2F41Gix3ocWBhUXO7m%2FatYNXR0z91tys87Gy6pdnkRRCRH1UZd5VMaqpfIaasdhKiP2eqa79zQMGp0zsS%2BMue2jkAmlk%2FiThx7zjxKrf71dvVssMOZQQynLIzwmgFwhbpCXbtrq&X-Amz-Signature=f088a3c4c0f46491ba9566dfae916e39bbbf5943b93ac03524f4cd54d0ac22d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

