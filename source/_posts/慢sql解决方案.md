---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UPUW5HD4%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBbbx41dz4DTkIneAGiVE5D9sWnhuIqHz7d5%2FGkli9QDAiEAkeHMZlPk8nbL6PQ1PtidQS6PbpDqBqp924MB57SB%2FNgq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDFhUP%2FIHgsvHxSx7JircA12TXMPLaxFzLcjPZ2tYL8pDE6UKThZmgHl%2BUFoEpPP%2BYW3Dw2GZQwdk2VnCM9BEOShmDvfA%2FnJx8mURX28uG6XQ%2Faxl9PIGc32%2FuQG%2BXgE7wAWKE5h5b0UtNayW5SZvdRQ551UdYeO4XvR8KTO9nxD1uYJOv03ts6LCPp7%2BN4XTtU%2FFu3yGaWJ7vJE3JCnzWd5yFWRxmWjRqZkEvBih%2FjMnfmW8XsHfRvmQ64osYhW%2Bfhis%2FK8JJVkDRevZI%2BLKBkqigt5%2FKIL0Fat8ABIlu2qIrkITVOXeh23nIgSk3Dj%2FyGMQjBEfDKTvy9uhAifmvXrL%2FMTpLIDSoa7prKj%2BjpQq0qWjyJF0jiJTnTeLYU0u5cGC0VRgaAb26zjup1yg38AxJWTjIza%2BHkARMmfULmJkXPLnkH%2FrkCCE1FEvb8vx7erZ4ZKVC2ZmU3TFh2cJzp1YLTH8w0kE104sBJ%2F%2Fmy1oKgJJiwPM%2F%2Bp%2Fn5YimvV%2FCG4cVnqBxtb%2Bv%2F2KDnI0nOyMSzj3LfUDp7QCxWzC1YM%2Bj%2FHBvscfT9Rw8qLOoLgv4zEHuRa9UqqGvS%2B51ClMN74jNiToc8Soozl3o0LSBKaxicrDd8TUmYl%2FaC9%2Bsu87JNVCtFfdUsrOqtPzMMyX9ccGOqUBgsoMgZ1Ts%2FJ7KiCheArkvjwhsA6bs%2B1rY%2FHTqTjmir8UWOmUxzZqHyIX2a0qmRnGQzDAXPIuwDzIRTDfRpCTEdBHnLJGgqYAso%2BXcYhaoFHBtgixvYb9%2BdDyN7J4lhaurxNeY3emQaeHzvGYKIicTu9%2BjKFXQKXYUxSKRLk5YTRYpoS0a0A2b9OpaCLlOiwgjvwrzyfpqXgFivag1jERHqEB8LIZ&X-Amz-Signature=ab5d830021bf72f6ffaf31d272d6bfee524173893e29c33133051662e8725258&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

