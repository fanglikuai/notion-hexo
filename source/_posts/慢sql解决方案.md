---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662BP7VFWE%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQC64Ws5HXHqZpDNczYEWSQOwfBrzmte58RrLQqBROT6mgIgWLA7R%2BOjaABK4VKLW8Q%2FJlWWvgnjKytIMpi9lM%2FhP2IqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2FkBejri7ctP0Od6ircA7FkN01U94xWmMwzby8bzffg%2BNzayZruskzfGgaXUiGgX0BRoxwptc5syYAWxDz%2Fn1UvlIVeDhY3oMG%2BT5rPN460Jd54uTqRLi7K9QxolIApWBGQi1MDpewQpExBMLHxupQoWNMVy23ha6ENR69FRcIPZgLBD7r6TJ%2FvSzEZ3rrq2dY%2By8Tgqy7Bx%2F8ohUAV0D8KAXHbwfOwB6Taov30n2377kYSWyy5QA1bKVBzYZGhDH7yHsh8UXOfkhZXhutwnCYttK9CQLBtyvMr7dJsQKWVQpXpLKuTMU%2Fov2MF%2BSOCnSvJEOUrLv4Uj1S49eJzuJXTmjWSDMtn2M8R4597RlOmmAk8D0PhDoNY66%2FLXdSTkbvO%2FqroudFb2wsArSaDk1ThWpiCT8Lmf%2FnMXNPqZqMmatfhNIaBXNr01bSWWij%2F%2BhLabkhtbTzVG41otbvbWAqn2eMdVKgEF20HhzALik1Fg4ugjctfYPv%2FQmq2bfZRIjaSaW1hYemgDGJbsQOGbYlK%2BvYiPxCHxwcHtIzg8HqzJ6vU7WAHSsT1lV5L944OpYo5jcxy%2FXnvMQMUxZ8vsn7GVPJz60StKAtLqG6vR0CZA%2FeWpdJ57JE4RKIMdCDG%2BBsJRTFJv35L%2FVu4MJq1jMgGOqUBk7m3%2BNLCBeul%2BjEHTf3jORwTuDjGA3bWilR90XaaD35PXjqQP0se8DVGLSV3TILBzZWh12cP9UqkYVjs%2BcxIIRgRYieRlNEe2CcthWugMx4azfDWJD0dgCC13UOVqg48Sc9%2FO4Thnwath%2Bif5CacgY3BxyXdmSyh99WVHayI1U%2BE0bWHHLSivrucP5GIwLrgSkzFBUGLrAyRPbhQglkugc%2Fg%2FXwA&X-Amz-Signature=d187647e818a5d57590ca4dda10b2ec2bde3ac7e5cc12f422a3bbd35282382cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

