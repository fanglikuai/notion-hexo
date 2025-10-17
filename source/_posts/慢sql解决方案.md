---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PNG6Z3F%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDfHpqbKyCJ%2F9CD1MJZ7SDM1o%2BdP3O%2FZj097Z9x%2BLPzGAiEA2W9UuPB7XxovLrNtF8PEqZBYwqm146q4sB86IrFNAQIqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOpwCNnqTjxEsisDfCrcA8F74QnDCLQ0ZTqU%2F%2F21p4ICkpFWORXDihJ0ja9SQh9BKInL%2F1PyvcyQxOszzfHbMCQofBXI3DrAaGfJ%2FkqI1q7m2eiYakPtYJPKXunc57P1hV4aohNGRq8CMQWlY3kt2%2Fd0ckSlpF%2Fk%2FefEaul98vJQp55YBuRNMhVwYs7aVUs0nX3BXfiCiq53X50uqGqykWtwKj1ekhefXmQ5%2FyKvYtuePW%2B5CwLxPm3eZxmgDkahFT0Ks676ABp5fMRYaasmLoypcPzZvMIDpYcdW6QYbGPS2F9l7HXeMmlJTPsQjKN%2F7sDy9eR3C3R2QmVOdewfKtLNi9nBRwyMLFwu0uZ2lxOE0Bt0lytABvunNVZAaCiBlbAkLbYBD2S%2B0Owz%2B9ITowVQXGAUaI5rDa4ZQ8hqAf31ZKvGjEPZjx%2BuZ4HCxV9bsurp2whXs%2BYRN%2BZGahOpOxeFYp5ult7PqdZ2bBe0jdoSehr0PbwuyW8hDvGI6dyGIDE1isWVkR8utiFPBOoitysGNIWkZP0nLvBw8xjw6pSUt2P%2B8nAkAnHp7S3tUo20ZbpQ3Wjdnv9eZSFu5aBLCrwRXh7fl0qAbjIIulxgxv%2FnFVBFJBJPG04ksCGPr9TPpDp9gysTAWdM9qzDMMbBxscGOqUBGUAlKTewJ%2BivNwUdTPLU2LI6ZmgCPadCGyz6PSKfLLOPyTTSYti9pT8l2eUSDsVJOXC5Ziv7BWs3CmIJkE2cQtAntaqVadQicLTxcfyoXAD70%2FRCBmAuAy3ySTJANsh2Y5eKV9kxZZZpjdE2ekFqgLWtZoF2IOIEaHE4%2FCBJpl7IV5BA8Er9dHnQ7Ioj%2B%2F7h3ixXKuI8i2jqgSdE%2FMHXxbJ6UMS%2B&X-Amz-Signature=a886055a8b70418d67dfa53c54910c131cda47f006ae9eb7f892f35104dbbe74&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

