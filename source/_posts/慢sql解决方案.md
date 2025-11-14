---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664XSAB4U2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDI9RXJvdszKaeROdc%2FdKaodqh7WdTVEcVggL4MLhIajAIhAOjyWRoVwbQO9I5W5XbYkfV1CZaLZcWJUvbTj2kHFAdwKv8DCFsQABoMNjM3NDIzMTgzODA1IgwK1gUi1d5MOyhPD3cq3AM%2B76NjHRt1I%2BUmcuImEsQ7Zak2GknV7z%2BcV1yfe7fVOJghi9xaoS09%2BfKqSHgrcfrwRc5JSRYH8CaN8a88h7Mr0%2BeOrQV%2FnoXmNLwv%2FNu7v2LjjtdfusqMv02mhANnUcdz7l6ztHQohg%2FZin%2F5IjbOA2Rv%2FYF7VN1szbtEoiBHQmMRSJ8%2BoQI%2FOCh6UduaC%2F6b0vC5iNLhVQ3iRb8067p1jOb0A%2FnfC5fGjnzGIvto%2BW6IlU0OW2faRZaAjBv9YMqNedB%2BUUdUKz97dDySovGOW1BRpGQ2XDRFDygCTMExJEpf2myCbMiO8dZrhyI5NiGkTEJ1rDfaBY9jicfDk9uDHOXDl1G36L4NxWAuhbMGvvwmJB97fPQedf4bap9AcFnu38CfNl6d7qKPycOhlQNcF95CCoqvXxzDbkFh3Hg7xuqqEVs6otnBbkxRLwA3P5Aq9xgr6%2B%2FIijp03%2FP0M4ZebGLaV2wCXA%2FYRxgNxt6I4cuS41Rtxzd0yMRokmlLW1z7CADDew9FUQlUKiaDGRPFjk1OBXZRjpWlguQkM9V9UoydbIFlZsJyPbt76dC%2BD8qeq2xXRHzN6TPmkXhiNKzytsQUvxc84XyohAYFxa2%2FCxT89VI5r9DkS%2FNClzDjk9rIBjqkAcdnXmpY2%2FR5r0zDJtICk1x0s5lHR0AsTHaguFQSjzS32IQ43OclMIbaogezN4gL4%2FhA6pyp4jrsiPTn25ZIX5lUaY4wqme6EVSLi64hKeMjVsSTK3NwTtnrfl5y94w6%2BTG403bS0oaSpUZ011f4yg8nKeHaDGQFY7x%2Fuc%2BaP%2F%2BSZEhPmo5N5WQHrZGN45hri0AjmZ2GdAOUrOvx9S6pEdnBEdmh&X-Amz-Signature=eb1b2b46cb5f872d18f81d7d3c580ca368d4697e51d3dc7f7f978f0af81210f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

