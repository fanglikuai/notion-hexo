---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QER3S5TG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5Vlf3Nv7pyOcev%2BvCgURIxy%2B%2B%2B0%2FfJfC3JZDr0t2alwIhALg9MRld48rDQJgbpWx7Udni68tFIAQi8Qmbc%2Fa8HpYjKv8DCFwQABoMNjM3NDIzMTgzODA1IgwC5K5TATTo5tZhXFMq3APmZN5iEl5zmJnOTqG377mM3Efd7KfgyrdQbLzXA3EemwnML6WBwhpoeXHRs%2FyBZQ0ATzkiaj5A682%2BcGDutsxswcsTYxN4EOf4t%2BzqGVtZeKTu2dgUqR8gVoa7OTvvRexgXdy6VkJIV1NAERXGWU9mP1dv8herVCf15VN5B6Aouzks%2Fbrbh8Z2GKzBaUbHgci%2BkTHAxkOHgFDNxkHQ7EWqnxuKCppmLF1YVCMD3miIdvzJtBIO3VBIlUSrRQb%2BNCfqqk9KVaXhFKuI27vHdOKBkLu5AgEU1LUSfp2eH6658AnLOKW%2BEmozx5jweU6PiFxdQPTEIsH%2B91ttF9CmEIrPtoEWSnTp1luX7xKBNDjWcBoxCy64Tur3oN9mWN0YAKSX97gVWmIAeKRya%2FovFTGV831SlRgr3xVMrXya9YOPs8S1zMxpP6G2ZaiU32gU9ekECuoLNJL43wmvzP6fev8BUfZQWGUVYnKT%2FTMoBM%2FLhrHnemBWevi%2BsVEEqvDTG0TMNax9fO3hmJcGKfcwzt1Y71hpCuEP19s%2FpfTHAvBXdtWa%2BP51j%2Bd%2FPcaL0WsVBB6%2FBNhjZ22VkZvrozOc6unM77HM3byugmEcASTkflewAl836Sn36XHxHO1VDjDflc%2FGBjqkAUegd7k2cuEtxyiWppBaK0agrZikyryC2LUhzS%2BVpJf5X%2FyEEDQIqpz8k9T5zKL%2FxwZ9B17sxGtEjFjeno1Qw7Ni9Kchd5AhATdFf1Z0fbrkGT7iFe8ca73t%2FR32map9b%2B%2BfeNbm%2FPXD4qpswyPl1GcOMfOo5dBHkU%2FNOKmLQmduC%2FquxybnSWiSApG54ASfqQrkJMDQpjMut724pU547lolxkfN&X-Amz-Signature=09f72ac9e05a375a1f9bd0762aff2e3da08529d9d13384ceedf1b5aff34cc161&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

