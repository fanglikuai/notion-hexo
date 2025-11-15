---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667N3MWM6U%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC8OCvgSU8YErJiLgQZNEe9%2Fd7wRu1r20nvtU9CSYMr%2FAiEA1HQcpoLsqlbMVca92gVEhiL%2BtdakM8TiGNJKsbzNYdYqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsHTw%2BTIwMNBt7kjyrcA121T%2BZE4Yx%2FcwUvA6B6SC15oIm%2FFx4LThMkTraC4NBf5iiAaRk9i59xG7zdQlKVv3YI9CtVZ4Nr6HrY8zszVgshdMCS8Ela1Rxgw7kQcxgaK80ykFugEUZA3LNTxuELxhIlOLIQ1K%2Bdn6UfaG1xKO2YQpUdA0RIpUZl9NUY1yS%2FdATXAto6uLLmcjo7yHJykys2zKKaJvJfcmDReM6fO5RkT%2BbRr6%2F000IaIZ9vJNkX4k0vmTLJOS%2B9fc0dNUNOXcf%2BHfw2OoUg6Xrhttza%2FSBFcK9P8rg0do8KfZSUB3kgA8R58rjWrov%2FTgmeUbNxIBvi1rRhQmNegRAuJwyY1koCqRvISYKtVh0Blk6mMf%2FKflBeDOdXL5BNYZWXAviskHWmqMiqAwvEgS6X%2Bligr5hnx6mQNcTGRCc3NgwjRbmEw%2FwSy9DuELmlRfaTXT1jWdjaZpD7qyWYmwYh4IcH1i7R2NoqhRJjQR1Kc9oGmp%2FrunvC%2BUedOlOPVBL%2BJVO6BF2O3aHY2CDI0qtOAxU2ZrBZ6QoewybQ5GRUmSLPQu5Q2p49RVluwN5LkbDDcVfJmRYYyHLBbRs8XWWLnQ5Lwk71ACw8hZoFjGMSZomTYTqpj83XdgV%2BubhkNWeFMLTE48gGOqUBfUZoHRYOCkSaMUwbczVdgMZlCZNrq%2FoPSRvFRDCG1UCmIGnhf7KThXGxtj1rI9ataA1WMgng1HkMVoEyN%2FszCK6IBnrvomT0igiMTg9pdVH5fjdcdh2Xsb3fslnWllBrIW75gGxTLDVOW3fEKvLDX0RpELuqFpSC%2FmhAnMvvd%2F41arQ4wypzu2UX13R1Zp0no9M1BQkt450fZ62j%2BdJ107OU0CuM&X-Amz-Signature=7556f428defc2bccd2451950f5b0db9a3f2ddb2d05997b2257b524339c0629c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

