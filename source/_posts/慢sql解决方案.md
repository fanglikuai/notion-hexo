---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662RQNLPAJ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T130112Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJGMEQCIAuJbb8DJHBsoFmfJUAq78zWHMX3B1F2h1LdEDMy4GY5AiBGWQCKcJDPLnV0zd%2Bc2KnuVTq%2FSyFMpd3SqMjtuZR2sCqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BxkJW7m8szvlTL%2BnKtwDBRiQf1FXz3ueW5R%2BFL4SRtlR7CB3vqrffR%2BXQHRI%2B1YjD7Kok6JtRtUbdzkdQpYjpe44ujIXmTrsQZWeS8%2B%2Bkcs4uHn8XdUdF9ZA4DD8lRGgho9NLdpQR2bTjGK2an2d%2FRThLm1r%2BHZAG85HV20KhBF%2FDaHsQsMctPh7PMPFUzTLTLQLEbRE1Fli1FrdnbWKhf5cLFCGwZreiItK7lkTNw76K6FQ6BUUjE4mh8Rqqc%2BQGriiaAHRfPHGfXC%2BoyoHdYN4NKiw05kWaVSO0D5kt0cYSt5DaKrEeQBRtA4b9XRLrkG0j4PZlQ5%2FReVSBSBeXpVCYR9UY3x2NczJ85JD0AoifgX3k6CXvL9K%2BJ%2BiUaXHBGeFerFL%2FWEpi5KD0dJFoZh6n6Vdn5UelW4Mq5tXe69TfSsNEKtDXKn7EjZI4jdSfD3YpMNApmrr4AUEFzhohEXGRP87YOp09cbaBbNg76cQAuXvXkRVshseb%2Fa7V4AhVoJJ0DKGK4gJHCNqXYbd%2B239enpfzXik00ZQTGKe6j0i3urd%2FhZzHxdmXHXjR2KH0AJoVnGo13Puj8Y1c5UQqpNLiLGyL8xUacrEP%2FhF390rN%2FUV5vvfFG1JNNOjSWX80oxiVTL1wJcu%2B7Iwl9K0xgY6pgEJQ8vx0EmEU4G0CQgXenE0RCtlzHwoxZibIcRd8dZA%2BnbXA5x47h8tQyYKXNG%2BcZauniNyCcOR7CoUcER0vcPm8pbzCGVxlO%2BIDaV%2FXZ12jN%2FfPwXXo%2BUJ8s0hK1Q03J%2FQk9fJORov%2BMTVFuqBgicX1XIiv%2BPjlYABY%2BZ6v7WKLzq1RAIZQ997S9Ouw5oWvtKkmjlbK8F9Ye7P%2BvP1TWjZo8t6dbNm&X-Amz-Signature=ee05b3e8a37ce36a322f909faa90aa055b1a0c528ed000319f0761f07ebee537&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

