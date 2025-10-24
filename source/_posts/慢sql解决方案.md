---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMA3SS3L%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7V80cxeTePp3HhOi4PkBrQe9TN3QPex6P1MDHTLOFcAiEAh%2Bn4IJe8dyQQY9kxwzruoqc9Q4yfFh14jLChX0sFvFsq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDJ25Hu6TjU5%2FaYX3KCrcA5i12u%2Bm7XqF6PKvpyMG244I6RJistR7huAb2G6QDdEYawCtboi%2BQ6mf2I9RSvNTR3axKJa2vSR6FLNqCbYQb7tSfD2C5cybuOq2LDzBnnKmmXH1qRfUXEgimLXyKlho14ZulfHOg3dc4uDcZhYGJqrCRbTD9XATeRmfRCdGSzFIuCj7GLmKQZNmsXjlmz%2FZeXIH%2B4DOkaZwVPiBUanWDuiRPe%2F4a6KTfX%2F4CXrpoAGHq%2FueaMURNfcRIqTiok5Z%2FpCaIg5cgg2pBqmPrPezmdj%2Fm7mQt36f7NAiSZskEIa6ZO%2Fsjl8G2ego2F8fz1YYPFCcIt%2FZ0JmXd4tLNxvHO6KmNqDGzuuoHHSZkX21DzU4zUt8TwIHpGuea4Gv6jPm4afshdaE%2FFm9Wp0GGy8CrFJ11vGDVDvHAC8Bq9M8fewVwixab6DmX%2FaR2mMS1Np0Y2%2F9xlfS%2FD6Xz9%2FIcdLBoqxlX7lqJ%2FA%2BwM5Qe7HoZql2oNR5b%2BfTm0ai6S8atKnO5vzKBB00Y4FAgxAkanRIEiq6Cf8U1JiSnkKBzi0qRhPFfX2I5T%2BfXI6y8VVfz9tliaKsO7IS5ATJ8H4RCd2Py%2BvJeaD1pZ3XoD%2FKbiSuqh93jAUPypaTXQJKVmqUMPjv7McGOqUB7qXWW0osjEu9cslS5xU5FWLxB8wOI1r%2FkbAI3T3OWhQHSI0tu2%2FsoMIQa47FY0n1WsZbYYtAnEWfCkAlT4n0LoKORYflkXqzuUMFOGnFvxwxvfExF0tTkeA4UON3Vwmwj1JKXFK8nHk%2FylrV2fEfDLBk1y4Db1hX3wqf1ZW2scgLdW%2F9PkwpxR%2BsU6bPLj4dJLbUnVXy%2BQxIw4GlCzZPZ2%2F5wzlC&X-Amz-Signature=bf65f35ed53e83abc9acf660724a7a3306a9c11795647ae2cb1fc89c195e017e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

