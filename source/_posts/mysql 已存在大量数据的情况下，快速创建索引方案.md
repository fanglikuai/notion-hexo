---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCZAVSO%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T081019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID4X%2BZTKiResaPMurPKYsx7XqPpWnBqj1%2B4VEej2a%2FdsAiBd1ta3zo%2B1VLdXbIRpumxrhAjKGOopcEI%2Bp3Hh%2F6qDzir%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIM6nGuRe4WO9D59n4WKtwDPSCDdep1nYnjAmGlFw%2Flm9ECrbMUC%2BOZLDhE1%2Bwxk5OW0nt1%2FhU1HhMH%2BooWAZrndgKEs3PoQZruAHZ%2BE9vPTYxbf4L%2B5Glx3cRWANzMeDurEQYQOYhZkjH56hjXDujwWvK%2FvWv5QLWBcHFsIKE5cCA9xkE9jAbUOByILvA3ttoPrgpBWncagi%2BEQlbvqGaLcAAjDn%2Bokq0kq5bjKFbHKFZqEszZlYTnDC6lGV3bl4oPoZsC%2FCkz5gTFHWVTP96qSMeIQHvRZo48y35LcWu8LqhHmWEBrfJezLW%2FWBqsUlrVZTPdA9wOmYBWJTHsWTDABG%2BTPNIvqPycPZeMrtlf7HgS8ELcHvGF9%2BmA%2BunTK2xYIkOduoX6dgNvslx6madqRypyYrK26MOxenbxZwGNPkLWJQlp5Dt0jQhO%2FiQEltuOqglvJgeudxZVD9V7Lq%2BbSuDmCCsqxhMTnFr29gkEXaBPXfsDzZShc9N1%2FiggfPUtun0Py24E890fFTCYitFHff2bv0nhY8dKy15D23yZtjc%2FC%2FbBOB%2B0%2BTgHfsR4wQUut6AcMmTemKEuPnDScEAoIGwsp0fpAk0oxi28XSHZy%2BkzyROmA%2F297%2FhDqD%2BhdLrxfkWP7KYhSnEK8q8wp9%2BexgY6pgGDPTINTNBTpMq7e0VDZ9NxQThBVNK3o1XJmt0HSeMDCWmmV1vGbeJ%2BKSvtKAV62JrgYAqfP6kcCsooOMwvXlD4iSHws77GYquljZB7SfAYDG92qrWlpr8RMQ1qWlhFxH9ikAPVm1NX51rOv4bMU%2BDxfYXwVXlOnvca4%2BcwjEFABUoOz3ZnVp9t44yglT%2Bu6xEBHYamZeLeIHEPsyawJkbK9KOgDUoY&X-Amz-Signature=55d4900b180287ad0ea3ffc6094e90c2ce79b286cf775d3eeafac65373311455&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `<font style="color:rgb(77, 77, 77);">create table text_assets like network_assets_blend</font>`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

