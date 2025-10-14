---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663HJA4Y6P%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T070038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDZ%2FQ5Xcmbcrl6fqLHZRtLvl2hrKS9BrycZZi%2Fy4wa1YQIge4RpUUC1B%2F2d783YHUnA3H%2Buf%2BOa8dwCh075IcU7ivwq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDLz0elLwoY%2B1eiWVQSrcA25okgTVBwN%2BMJP250x5So44PobVGxEc1475hwvFtB6ZPGbCSLLgAuCzu27j1hytsrB%2B%2BynPxQBaJW2QigYpiFb5ZFDbblArdOdy8rMBOHOHtZA1T3RuTU2kYXTfscTpcMiQgUdi0%2Bupg5VDx%2FZULlV3KYcQpBDrGBoKW6nnBIm734tTWEkCtnHmgAWOtqav9BLYfGvGXstz%2B32yyP%2F87IgDXVfGAUQqZQTCQgb47YezW4TT8DzMZU%2Bw9k2MNac06gq2a8xTimtOXaiNX9%2B4hWw3C7HY506ZHsgomjSm2M22NaC2pjb8S0GllHfxrO5OvI%2FQQXWJCXNS%2BF5XuY5fWqj3qYbju84KJIZucHtFtVtvAY%2BR%2BqDbcnMi760VwPIUZPzCbswmDd%2FXJctTAFX4B%2BtfJoWjzrxuyqlWZdMeeevngeumszEATP4LAel9FFAPUWl1LLNKAzvJwDrp0nPVSo4TvsNlFXtQ3Tjn3j1zR9kAABJ0ebn3w3JLrFtnSFOSEZVFU4P0A396U5%2BxaMSYonL6gvSc5k08RnNfVmsazaUGVlhB%2FsPCxnuNdH6AQWuc7pgYif9ahB5M3sGJSQQu9uKrzkos0JE7Ll2IBK5gEFpau0Qzv%2FSq86flWuZrMPret8cGOqUBRJzO1tX2kdfXp2KMVZ%2F6t3IWnaTclM%2BUMNYH5oS3xhTen3MT6mP0fu0IDMmPMSImmoHQkh%2BzYngoqSOQO48yL2SJTC%2F9HyAuRcDPVuimv7u5bZHbbRY0B8lG%2FIakSgBlmnIi1voawyWEoAISpkLL8eQLk%2F8D7Ql5YzLxoG7AL9d%2BGUl13SJqyA7OB5z5yJ%2BW9E05A36sWkZ7q8grZdKgLWjhC5cP&X-Amz-Signature=22ceac0520441e048a1cbe678a4d040a8bb828668f364d585a71badb0b956562&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

