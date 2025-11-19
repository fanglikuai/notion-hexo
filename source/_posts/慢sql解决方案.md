---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHFLRMSN%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCICrJ%2BTS7vsk1QUiJRkvDmvxCN2VjueIsINRiBW%2Fk7O1xAiBBsFlyy3K4oYMHobZ0ClzPBhyUy8VkMJDpRZezU1JC%2FyqIBAjj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMMzS%2B5vSxi9sPUHMpKtwDu8pj2EBvj02APmCyWQTYKLO4Rp7wavPcqirLX4Mb4OGvUeGpBEXdc2DM3091UVWBiI4jf3gzGT53%2BpWDYjieU5hPM8TXOUWLd0QiHMi1mLXRo5OxVjRvAoLZvPt7AsHmOPj4sqQdyvQO7zZpDndsa7ZDCt%2BB8VMjPjoPTKnJPMTcQvo9M2wZuWSJtdRDGPo5vEJJA12cXUdLqBTQulsqDMyohu61XG%2BhJSicztICepIpyB2MNGTPkoeIcjo60JWF%2FPAw4LA6WMgND9m8EyKrzhGOtIroIrCkxlZKVuOhXG2%2FuWVSMTqVQFa3MWaY8HwrwiVIwEJixzzYdUcUt2ZlQH7svDnPBKprH0KUgJn8C%2FBgDH7seyf7Zk%2B0iMAhgUlOZ1ZZQlDuBcccMxLmbQ5kEUo%2BqWlBFEZ9gJhoj7YZNzhpQrgRQEgXksVZvD3h%2FClzfVasqy%2BnK6%2BUeTvOyeLiQxHtYgQsmrSRqGXTEYK%2Fsz7hIvKREIFVaDsr6m%2FTlIbFzjHtgFodHmdrzr5X5ElkywDCBJEHE4Vx351c4x2uA4tBT7ePLDTDxbDTynx2N6tfx%2BJLfdDp22Q07DyJtcmIhWaGxbGfPQxHoCGNOXE%2BQ2ICkEhUpqBuo71MEXYwxoH4yAY6pgFW9DGVxlJY9rT93UrwCMH3LCtntbMwkhlzdvSDwaJ%2BQowJnd7in1tIQTW36CnhDVUMjZe4iGMxpl%2FkrOmmzbNL%2B%2F8ORRPpL1l3OANsXrXAgnN8ACNySk%2Fa0rfXKUW%2BaWGuMs2Eh2E5Cm1Np7mjxVEgWbBubEbb%2B2%2FhcryIEYd49tijkY9c1I5eLRjmWGNvMs%2FzqexcByx%2Bmtog%2B0i8ZzcXg2ihBv%2Bc&X-Amz-Signature=3e886a3b31559895019ac66b5c91b5e00b1a39a4f92d85e1383148a78c2f8a5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

