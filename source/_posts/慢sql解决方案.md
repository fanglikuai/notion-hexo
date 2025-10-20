---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7F3O6ZN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T020048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIEuBGhWa2i61YpMYmHTQLv6PGnUSH6BLXZYPNGOsWfudAiEA38XgOPkd8N36Oe8Bmkzyl38a0P3oXJmAlykmjHCtqqMqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCl86LnEFxYGE2bj0CrcA%2Fp6BysZ3L%2FDMOqB7jJ%2F8U2pcrvA2UeYOlgeftiKtAUmCMgwiW4bIsrU%2BYOMZ9QFI3skhqE%2BIK0ZXQmJu5l%2Fgx%2Bhib4Nlp%2FiQnH9D1DRa0bLJ2SFOt0MjhxyTeWc6v6%2BXWiNpxOAWRVoGMF6VZj0N%2Bh8XVMPzIx%2FzN55GeZnBbMp3gPzYBQDFR7CT7Oa%2FQ745Q1Z%2BWUaz3WON4vKHtMOPElD9LZO93uj9MQpsn%2FUOHbOTgsyoudyXyLWt53I8byYOf%2Frpphudnb6YSjtom9V7WVoi22AVeVCvO172mRRYBosZQwNGoSWYOuu2%2BpA390DrCFgkI0rliKWe32fhTjzyUQALL%2FpbgMF6nDG%2FL1sweCNMNlFqFiEvxzVKsUC2eX5Xcwj2P47Qafi3a7IpzyrnR%2FZmIM75hklIvQykOOgsx3%2FSywojYEUxarVqdV215ImIDaRDb37lBflYUeUV6nutQ8oPPOlxeb%2BaVWP%2FRo5cAwkaOVgSyHf9H8a3dqR3%2FgkovQsTD4t8FFI8u%2Fc99BLQk%2FPrEHko30fXEKk2i2sw8Rkt70KUqSQ%2BJvvLiS503TUIlLvpWxzbH%2FZxMq3xTKwS%2BsIuAZxYjVpxZqs6mdy1osqzdHL3a2A0EVPzb%2BYMLn11ccGOqUBOzQfyqrTlAbI9Au6uZpf18neq6xBDIVG8JMZOwNMY1HJsbgroozMbsGGEOMOliFvE8MG9EzEpY0o%2Fz62HSY2NDUlos%2F%2F7jpq2%2F5lVNVbnJ7V7nO7e%2BfjQNZ5vUdhWqprQtvPah0CUUu0lfJYbQSslG%2FsLHYPmhwMTkKQxm1Omk1G2Do5IDumeTlNh509sDPQbrG76FOz9TlZqshiseTzXTvPHN9Y&X-Amz-Signature=b30ce233cc0e04e331cbbd2d1b98a9940722d069206ed043831cfecea90c8037&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

