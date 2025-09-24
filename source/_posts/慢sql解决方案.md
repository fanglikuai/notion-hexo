---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPWLAFV6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBN8AOx5t3LWrxzsmGXsCnKEALDhBdO%2BvU7JYZZ6Jm36AiEAqdEQfvurez8ptgZXgDqTzalSffmSK1ABOvH2ZlB%2Bxjkq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDKyXo97XvHCwdpWvMCrcAz5kOoIE4rehas7yU71omvLOd1pT%2FPn%2FqpIhicyOdeAz7sy9JgyRou6fCmzlbdz2skQeVQc2Avv%2Fg2S4GqUI1px21w3XQAqKOjdcQuwsMsYyD%2FOzid4LHbZPAFHXU%2FJEV%2Bh6OhEeKF7dHdmKjy%2BRiRkSl3ULE%2Bss1zCPZ5wXjluMvw93HaBYHLQcNWpnqEmAvu6WAHukWDJa0IdhQ%2FdDMcekVXYCyx5I2JMvd5rnPIctG3HBSEKmyVEDQLC6L67fiVUCYsi4Wmp4aJXp6N%2FHnymBA1O5CgYmS44EazmUi%2FxUYElbyhTbfkk1NAJfVD%2BVD8BFlWfdsTLn4dux%2F5SSh9H6myaxsDRcaTA998jY69dPZOxWca6zfQOcNrHiBWeurimi0U%2Bh6wNEcRprXKsmzzHhbPyptPYUja8Onc5gblDiB%2BEHwZT5jJutef61UP4s1%2FAU%2BGX%2F48Q6pLQ7XH%2FJQrP7A5CjKhlUpOg4PW04HoMhBXIFUkH4o9mQbXJGQXXAEBWiYNYuvsnGxmx%2Fd%2Faf8xN9EGRghOPXt%2BpPboL3j1nDLPN8gMzhKmBqt%2BtyUw7jt%2B9MTolbx1L%2FPuvL5TtfAfGm7yrCZV8DEn%2BNcbNC7cqZRyiec1VpgPYUQnyzMMmHzsYGOqUBlpnhaYX7gh2e5AU%2FSZX%2BwnXiV707tOUEQat63N9rZ2a0kmqpGXSQGsHQLpaOEPUoLdVJY0cilLh%2FDSS3GXZKsPmM64lKcX%2BO6iSHDuQSbt6egMrGlplBHCULo03B3qv8cVLclrAXEG1UgaW4QCiaZglcVZfH0q44KIVvvXbnI6ivQQ%2FYmhF3kaSXnMqYFGJ6X3rVn3NhhaAdYrMDuGso%2BmiuRm4o&X-Amz-Signature=6a1eecdaa44fe80ecf664e6cd4dfc80d05396af4da4ec472459444f24bb0e6bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

