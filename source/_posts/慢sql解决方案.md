---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UV5R22JW%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEjwjjjZFShYbcxqtNHF1fiC71KdlEmVtHtZ8t%2F04E3AIgKkp2RsNKHkNPa%2FuJVy%2FUBgrb2LHjCu0lTvpTvjouulEq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDE2JmzA8NPQQMGvj4yrcA9VLtkumXYd5pWQpt3wTaWaNO%2FoPFUNiVvtxaLPbfWSUO%2Bwm65PiXi1LkMxV889dqQY0w4naTPApVz4UpyynfqFGsR0bjPfyBHRyBjOzeh9VE%2BMsi4fVFtCnkx86AL1zaq2%2BUePZ%2BCACBl80mnrwjK9UR1Xnadx%2FRnyaaayO124we5M5iNF%2B6mJp%2B3aoU89kPtf7mBM3KOCiC0cp%2BRXkdTLE449KTdSBSmIve2724VY7GQqQs%2FxJ0XKN3mHEC6J3mvA6Hrxvj75PfwBcfxTNfQHiQgIL7z7ESEVcWu%2FWjEkXyMOundAnyCSIwdhwtrpK4M%2Bw5g9I4qi2GzQ8EnC1DkBNsYk7qv9hozjy2JGgoTaV1HUiQg1VEMCpu34dH82QvNrxgkEUwR%2Fz9Fc%2BdEpqZ%2Fw1h56SSFEvWzGM1i2DQWEHWEI5Ah3mqqjvQ4NOSRjNh%2FRmQ0yfanuC%2FRfycsm5WRC6zcRwaqCRFhj5BdVvQoXu64XoAbkTDCHzY1N9EClckxQc19j%2FG%2F2%2BHBajmgKBl5ijBN%2B6B3OjiAotJAc6ENmeapg2lugX4wSVXBfkw2D9WmjUK6t%2F6Hrlc%2BCLkjUJdJ%2B1A4nIMGsmQ1WSFyNemeBxGRJi8i1Rddou0ImOMM6JvMcGOqUBUelGcn%2B1wY8yryarkqv%2Fl%2FGPol9sJUtP3bOtdxjpaeLS0Bc%2F1VplE4CJLNLlIE0Lb666%2Bz%2B7OX0gTcMCZR4RWlv0mMZqpIcjq%2Fk6YQzUw0wN5V4MeGx7pa8st7QmihlTjj3MoxwXyZJIVaa2Q2qQ3uqi4k968Utwl8QNY0l3pplhrkRHYv10J5vbJcszVYeZwiDsh02zl4PsNHj2ugAmwtSJT1%2Fa&X-Amz-Signature=ba24d5fc5ee57fcfa5e841b4f11a583ffa5db48a4551259814ff0fbcac7a8f15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

