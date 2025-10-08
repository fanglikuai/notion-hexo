---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46623GEFVMM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T090052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIAnTzovZI4oxQ0rmDFN8REjClj%2BGnUf3dUJE0E2CeEpoAiBzurCAFHDrKL62SwaPclPqroP%2Fv1Q%2Fgd1FFSer%2F0ByHiqIBAi6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMRPzoKjO%2FFqYeEda7KtwDmaLdt%2Fhx9nPhakb6H9DRWf1ve1%2FsCD%2F0A6MdKh0Amp8N4d07hiExa9M0G25Dikh3DRFcNQVQU4SIfuIiGCBOPPygzA8JEcYquIUVeshpMWIehNYm%2F3BYGxcDxoncAt7i%2BLsejq%2FHu%2BA5lDRj7EhHDstd0u%2FmIrx99M1OzUYCInNAiikIU6FK2gXiS%2BwBbRX7N%2FIYrAkVIsQLR4Ozlarwakv05aKtniS696LPxl78ct8P6j0fi1L%2B4%2F%2BhE9j5hOi0HeAl%2BjxlKo9gcj8yRyD9DUB0aGL8Notm3cwf5QdeY%2FQnFff9oKwhRxGyAFI0sFM%2BvOzKud7Uyy06Y4XJQNvxna8HYG04gfO7xmFBvMsoInvsTiydpZPPrbY14WhA5DpX%2F7dKQA3EDm5%2FJpEhqnh2s2eUg5YuOoki%2BbZOklhR2qsUJ3zwzUZoOoAy6qyXIbzp%2BXD%2F4eDYtY2ZfVVQSt5A6V7j0jVJdMiGDPUCMjKNrro6bWA9RZktsyySpCQXlTitxZLOo%2BOieXPQXGyBUa3PUvdpJay32%2Bng7YGm6ro2yPyfftdIc65eSK329jsEA2LQKahIiWUPVp2cCm3SeraaDg4ZLwF74Y5iDbq22t3%2FmUDnRNrD4t4tBWBe3V4w%2B8GYxwY6pgFHIISz1mazwkjTNZcgQfVjRIbIiKlx1dP4sdKi4csKdx7Nnwn%2FSGVFeIaK7gOn9NfHWhMb%2FJ9PvzbwC5uuL00js9OsPFaZZ%2Flr0qsm0nh6D%2FcYiaYh%2F6wDSpXRulausrVkjZ%2F6z2IkoMIaboxKK3U3cGxtiR%2FK%2Bv06Qnhm5BU8v9eH5pHbAv404ij3ihwZdd%2B1jp6zdzGO8JZTBfj8bwkAc2PocY1M&X-Amz-Signature=a74092ef3f5e0295ca78cb9b1324d252e7f2f2bf099acd1d23de314dbf0ac092&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

