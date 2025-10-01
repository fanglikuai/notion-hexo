---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IX7LHLG%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDw5DVeWCs3s6MaqR%2FDa5meA0TOYv5o0uG5TH9dKF6EyAIgaC17yyrIzd9U2vM0iEQD3%2Bqpvdl0ojz5Oq2dfcUE0sQq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDDUIjI0Glz0kEMMJ7ircA2FqjQ%2Fw0wCMjaRN9q2krv%2Bgi2XGerCYzt3Onh2jVyV3X298bOE4IgIyyvnd1b7cfdTVZ9SKonIxvgLIYGv9FV0cVeleHXKPpZHmDenkqaPLPfknSwTYAeeDBcY1lDKLgERImVV91cG70lGYKJdR%2FeSthRxLypdveUxGqTNsrEpJd%2F2pNCOOatPR%2BzcLtyPhzkqbq3pHMniygYBCutFe8%2BjplsNSXjLEr7uX1IC7qydb%2FU249Kr7lrXwdxXK4smN1pDyb%2B55jruMR%2FXcOMFEwHoCrQLcl6ic%2BlX606kJ6LqPKmpoxmrLV8Ll12sBtcI%2BdswIbaAL9v1d63vbgf1YaNmBT%2BB5JlJF7VWXT8C0qaFhYz2%2F25pg8ZCUVY4nVcILIxjiYSUai0%2FJrjFVQybHlDxVju8cSmk3sT0FFXm6W30tReOqCZxhfuw3AQ0vSDfaTbUrR2VrILOvIjmqwA4eOB%2Bz9qDFoiL2xSsXngPT5wg%2Fs0pn%2FAWv1T6DKswmcF6VR8zEkE9Tre%2Fm8mKYz14hw7CQ7CRNS3tFUam3zzpH%2BLQ1tOT4A12dvtIXtk%2BoWBSSNKz9abbc%2BS7NxJyR0Cy0FCNACkjMbx0VC7MaQBbVlm6O6toru3wPumI7C6RxMKiy9cYGOqUBG4MUqYUP2iwdsjnFD9MXZ8EiYfg5yTgOvPDYdMhrZoHGkL1HAi215TKmN%2BM6wUOkD6RytYrqGhwVQo1GjPXE91X6vwF3QmjyhULFajtxLfmG7ImhJL8dX4IpuPu4k7YZtGZ3GmPm8bszjZ1oxLgIm%2FpJednISElfcEFtug0p%2BGzr9Da%2BYGz6RLPUGH1IGF4OGmoAlJFjkQ%2FXtvsjQPKbIHM89TUa&X-Amz-Signature=6c4a1387f9e96046a2a57e19a4e3d5d1ae59b8418b401faf97d7eccc5059b994&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

