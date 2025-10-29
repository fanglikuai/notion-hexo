---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIAZHTRT%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIFkLKS41DDR1ism0bM6WSWeb19d4yB5MOxnePYNBlIpaAiEApHLnsgFNArbTY0Ha02Tg%2Bzllxs3KsuS5iuMxmom0nUkqiAQIzP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFEsv4ALLCZ26SDnWyrcA4eajXcLSq3PkpVYiB2L1i4t%2FeqronX5t5Cc5bWaWK59QbuDCLutxyGqMN%2B%2FdNsFrMmb5gbsEoIlecOj0jEVBeW%2FWHQfcQbwD79NNLW8s1F3N7vIimHJJIfPBPF0jXXJYvD4W4CHE%2BRs4KjcbaioiLcAqC7LP%2BAJIIjpKvnkG4%2Bj79qEt1avxv2EvvyZ6%2F1pOwp3Ix%2BZbBIHQf6zSYHho%2BnscJp6rsTVzYiqbIUb4ZrDz7jLbd%2FmIxB%2BJwN3h1MAuHNDaB5LZD8V%2Fbogt7fJAENlLxLB6LQ2GO45ThtytdyFog1%2Fa%2FJKr%2FFZGQTmvRDYCOMUr%2Fqu5P942WZ61Q266F%2FRSeJcgK25YHuh%2FeO3wIqGhlulDcl6jeZEg1Ic69tOmMb943R5o3FnK1YP1521qPyA9f4iEYme51%2B74wyZolA87%2BiwmDkxUKyAeclrE7AehBX%2FpXCTf3oDe13oUusFZ%2BJ67uR696KfB9ES47H7U1vABCYKBjns5qv07E62BmzAVO9tBaycVfwd1gZlFVWX2mqOH3%2FWfjRj5fR7N1HUhACiyvYDUP8qlWdzUfUssc7JU2PXYBbz6%2FBkO1ChcFv%2BtNxRhe%2Fs0vHx%2BoBYsFQodb5rt94j%2FWFeQBNtDA5lMNWEhsgGOqUBymVdrFl4NNdHbaWhti7kF3279Bfq4yKr1Dk%2BFOTIfPZWj2siRLRfhpUnQ%2B33PbB3pwxdRsIZXXa%2BOdotgxHlbosMrt0Bdoj%2FO7KmTn1P50l8zcY3X1VI%2BJ4anQcIie%2FvoYxFksJEB7gdRvWg8H%2BpHUnIA1ip%2BR8%2B4DhTen4kbaScqgjxu6JYAGpus3BCg8cYDBAnJ6gmG1tbKVqAiInEHP%2Fmzfph&X-Amz-Signature=81c4a1a221fd8e32e0e1469cd4b32a998c965603d02a6c179dab0a0f1aad3c01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

