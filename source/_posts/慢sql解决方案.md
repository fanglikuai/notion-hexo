---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X6XNV5K%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrWv6StsEKVow%2Fm2Xg9FaWxdGMO3uGHfnsHNnxqlB4tgIhALkmgw24ytIqn%2Fb1F0BqLsjAcUJHQ%2FWIAF2otJ38RppVKv8DCHQQABoMNjM3NDIzMTgzODA1Igzhu7xhwCklIC6rZqEq3AMyBOzuVzOFwgdTdYPXzQLgRY6%2BNpgmC7LPwuvp5R1fGA%2BX2DRkNh8BNAMMYOvZa33BsHA1rsnPeGZe7wEBzkrCwlTCW6r0Z6GyvVrl1IfxnR24Sb%2BuJY3Lv2MydflM6rPo2%2FQBu7EgKZAXVwaFeBMlofDRe6zr0x41IFyBoxztu4mkptx0td7ZdusvdqDmdtpIcfcUzl60unoAh2nkWGECWsXE7MqN9DrfhRKgRrtYbPcM%2B2w9RpUq1Epq1Y%2FmrY23vxyP9gGLjho8Ol%2F0kZxNWq%2B5t6sPJ06zANA3CvFC0QP80A%2FVMZ6Hy0a2nB5gDrbLBVyVZEJuZFbQQ%2F4evyHiNTjd1aTxs9pHYPLXdLdGKTqQh4u0f18I2qL%2B6oYV%2Fdbp8T8p%2FtriOg2luxGD6wIjW4ZDKXndt8P8DSURbqCyT%2BSfgPROz2Scbbxi2Ff1UZjhfnQsAqnmN5XH27swLaZtw%2BQqRdyOMZomV6rrgl%2B07vPQv%2BB%2FbRe4NiPKfOuxTEpLx6x4qP%2Bxg42Doit%2BhHLJZLutliEk3hehTttuS6UHsVrxU%2BmV5eRz4aXk0ikFVvkSMJA4%2FMQiBRsgGzWN%2Fg0Xwrgy8jsjikto3SUGs2hkRKVaPZpFiRPr6Wd7iTC4vdTGBjqkAcb4uOqQ%2BsGiS6YtMXm8MD%2B%2B2LR05NgZuUoxU1zJpiz7Qbq%2FjzRSjAu%2FqaQfwWhtVEj1K3O5PD%2FZ%2BaY8LkYmbPZN5t6k6gLMRcsjAndFukegNKpbfG1%2FzNC9Z4hRezCUziincjB3ZcvaEqzBZXuaW6q8baJC799YfWuQfefYs2exLfziEgt3FxUhTxAeS2d1iLWBho4sVNmb2JgbaviwV2sECw2R&X-Amz-Signature=206ee4cf0969a3d6f45620ac7ece9e5ca94ff6761e5ed87788092c83e785bd45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

