---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YYB2JFZ%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDUvNInfEHOZamrFD%2FN%2BjlqYy9zC5kqYpx0L0%2B1uuuZfAiA482YKX9iC8%2FD6YaXH8DoyL47Dc0N6AILWhjkoRrzKgSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMH5QlWlDSh0eU3GkPKtwDZqvFKZCDVbdMEdvFA1%2FrHDvPbgMKcx4FglWayr%2BrCG5hwxElZqorzkHBH5x2spVkZNuTgk9i8OiFSPXV1QGGHgPQ89cke2HXmBxF%2BEAqvTrVp8EGN9poO%2Bfly%2Bu24YmVnA7mqBe2EGMTos1NEUY1agtWujt3Dz6QMcPNBuEk2VPyxmDpcGmmAvwZCTvcsL1Af2he7tj5XdAjFyV5PV4J88cfRlqRHCtdzGc8TlgjiaPu21a0D7WQnly1r2ntEpe8p14OR6wD5ej39l8u0%2Bmv93fDyCz5xMo8gpwmJUM08AWaaxqtMHqyF8KwIB9kltZ2INZ8Z%2B%2FmtbEyec%2F0Y5nOdr77qZP7tRpqDRvJbGISuFMX3QkDjXN5KgWaUBtdx0mRzbvFDevAQFAXDl%2F9baiC1omUgrJXAMkjXZpdBb9ajOAiFOypDT9fjNt4hnEnkN7QzPvoM078t1fr5I6BJo%2B0ENtOz3ipsYtDwwMtyRz%2BmEbj%2FFFHy6Rku9jla%2BQ9I%2B61sRYel01pt%2FfJ8VAFVbL9O7pcqffTotUGmZXsw9O03Cn23S%2Fm2zGgK7HXbSCJVJDos%2FAURlzE0hGiGd4ABo5mfLOqQwWFevAPXNrzV%2FbqsTPH9K07AO4LF5%2Bx3Ycw7JzGxwY6pgGusz4TJSFpLYzccdiNoazb07%2Bz0D6L%2FD48Y5yls5%2BojSb0mXd5vhYi65P%2BAZIWhDdZLklorsBPuOh2iXEpihluqtmkjR7eq4Ow27XaKpnCiSzYjdiVB6MW1X7sbk3eytYaS%2F80UxIn7QqB8uR15P%2BCVh5yCK7O3%2Fw61SotCfezpHrh52xYFxR3OjU%2B6QspQFraV7BPhEezKRBxYvtKBgIT5MjyUfhB&X-Amz-Signature=6349c213a2915397d40c10f216eca035152354c5d61cbf17114745cfc71e72b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

