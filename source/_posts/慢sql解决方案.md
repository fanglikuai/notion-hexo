---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYR7FWER%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T100104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcMrbjshJU%2BpjCn78HBNndRWaG0K4uidiCKwn8tKS%2FWAiEA8YGHNTJwD7%2FChel8g%2FmS8dPg%2Fp4jyqQcgBRmYeG8q5cq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDC2V5esG6cy%2Bvmz7tircAxGVxT4nkgbPTaqyAXpWykj16yJOJAUuuSWbsfEuT2HEgnxuYVRMhKFpd6U%2BK9N3j4AywyZIq2G7Uqxztl3dbFtU2GwDzPSzcNbyZbnCX%2BB%2FOEKGtopRsNbE9z9ap2%2Ff4al9FmBMQ8y%2BjagHzWe%2FF%2FXATsmfewhyGlC1KTih3iQeiWWxDHMO3oTiw8My2mvpYI7%2FIUHK73Lnkwn2kSk0jMAxmCQjfjlTB4UuEMGOgz5jgiSt061A0r8bAhtEiuxMHdMMqugSMtzGKYkhG5qQuws6qEioKjQLBkzUuhA8eJc25R%2FpTNVfIwDqxkKSQTSmva41u%2BcS7Rh9c%2BxJa2rnagNdkxrxgxNUZel4LB0sL2V6geFqoMwSvp79pBTBvNAEdVua8EMrHAGalGWJzqeOTqTJJtW84ABLPWnyjaJUDt9UJsxvzsTF%2FgUgUb4lygdYCsPobx3TNADtNcNdToOuaGump1DJCY24UyWz0wZCziqccLL9YQBFXlZxVlmyEnTeg4c3Qx2qb2wm7BpFkmTD813dg%2BW9BrSPVxOr15WcTc0RCRQXWi6y6r35a8jGhnYdTL0xA5eRhCqxGWuhB3kpIKe82rltaB1VoIBr60AmHXwleMqutSo2IIffgFiDMPrZlckGOqUBktChLWl%2BzaDEOrs%2FJJZeJpHpPbfx1aPdJt2o6EPipYQWfrmhIpHul4q%2BD3040Bff5Ml4DSCDeTa2wugnoMD%2F%2FSPQg0oBk6N8k8WvBOQ08TD%2Br%2F5aozFl5Wn8G8Covs1JlUr1uedHISRgWNu%2B3tAKPyLxTfTeDDpTj3%2BgT%2BWWa0lRqCZyLhc%2B%2FnVFOKHeHiNb6n7eV5pzPExdO9Dds%2Bs%2BZmyy8I7B&X-Amz-Signature=8f31e2d8c3ee0092d5cdd0186946fc8468b2dac3650ce28b26f6772869e5562b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

