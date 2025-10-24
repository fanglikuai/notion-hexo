---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UWNA7U2P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T140055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGB%2F2OKwMRQxd8DrIAZPYA%2BEgah8wP4k4o2nXoSSJdKpAiEAsPWNc5QyFNageNBmkGzYQFtIfvDdd%2FlO7nVjoKVryyUq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDBZkZPM%2BzcvTu%2FrVjCrcAwe3UGgABUH7M2BZyEkwfWakziHU5X0Vyuz3kFcWzpLUsRmHABE2ctKRqhxTr5dGTtd3lU4H16y0%2BpL%2F0j9XQcQ7L0%2FUQxyK01KbKmMXZ5dnKiYKCv95LyFdmwXW8%2BurluppSlMPWTLn9jOo5v7WEO1DJQ%2BpLPVYoTdO6hCPSWCWaiH%2BzU4xP3wehfpfSEhQj%2B%2BfpDrqbrad2FZ%2FmvveUaGhSsLx8FhjFSLbyE08mmoNKMnfSy9q%2BLOHcwhEgG3d2yWtOVK86%2BBbeQFupSNYA6OB8kgpvTl9CYdaKj28n0tIL%2BVQDHcWds316qdFDrIWb6R%2F0X18FHNAzzr2jBto4tuOOKUD4vy4qaciHds2%2FkyCp1uVV3T0LvwZM5nqCiay%2FW14%2FaYk7%2Bvua95fF1oCgKLbTRy6imddwylePZeQGDkEr%2FDwMaPUxRVAecMWMM9qpCTSSrzFTMMmbOrtsDHuU%2BAiigy3P7KrK6nSJqd315A3DwfEnVk21i%2FKviEqB15r4g3fbBGiLRgm4zPadgYo662gKQiY%2BqBavmJllBVJ6VP1O6iE46B0GUeT2zcczIKCemLYnt7st2oSS2F%2FRHG2C1%2FQZMuSMoAdyWVj37LnqUmeNZPg2YnfEVfGklg2MJWL7scGOqUB6osXwvAjLgYeUwH7lXHc1DCwLT2MiL8DtadJub4KYnZVcoF6MPOkI%2FU9QtemqntgW6mfwOtzysAkx7mgnd9QMkGVoM968hlFBlHiLj8Tvzpf1ApaXqb6564S1zuBvCo%2BjeBN%2FEYKjNVamhkY43CIScIL5ESPJkevHq9Yb8ZH%2FNkQjkWNuH2Lu%2BoJYBeMZ9Gf%2BqvFDXA1%2BO3Vk8Tbf3nqdemOL6me&X-Amz-Signature=175aa91304229a752ca472258c5cb8a6d1bb0d984ce159932dddec110cba822a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

