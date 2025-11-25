---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QTX5BUL%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCBMquF2L3bZMOpjKvtndK3O2U5o3foR%2F2VzYHL4VOBHwIhAPt8AIKaKiD8t9oyrP8LXbz6TNt0F%2FM2y%2BKzFZCVOgFCKv8DCGwQABoMNjM3NDIzMTgzODA1IgzMowP2BJkG59oJsKQq3APtUQyE%2BcK6CoFmeRjrJImP12UavmDA4SEsAX5TXIb%2FMulsoqNuql9JTA1W%2BFWKQcRaI7POSBWyt4PXFb8FefPyKLDMuQQKd%2Fe7ce6Ak6nvcgQ8SA7OVDaWGOFgQgv53pkKPROl8H0sO8qh5w4dOelJNX0lgN%2BHRqPm5sKJJsVaYUr0QMmaA02ElKg%2Fv6fVhOmTpUIrNfysPP7CkYHYGsPY5pawQbZCLlZp4ku1issIderLEOk3NmYs1%2BSqE0lhz5YT5CtxcqLphP%2BLyNiFrCHp4%2BbpP3QNqPWHGkBFRf1NbeG6CK63YtmKASV9BvtyOPcgvdqyui4jr5hW%2FzqRQBQXZW1mtC%2Bmryzk4sT1a3cNtmiSr6yAF7%2B9Mq2nPiCjF9LbrX7WgSiXraN7EtMYXqyj3K64hyBxNbWan%2F7icLKxK4%2B87%2BVHki9XDDLGYhr%2B6ZPfUdDp6FSvCBhQFxjyZy97IfXkMH6wDF9Xc0zA9PjBnv3ErFTHgGz%2Bawze7I4B1Vord8aIEpPnX3ZXDZOrdRGNoRKL6L8KuZFThy%2FI9yf2eHiQEvQN9Xqu8tFM6Ko%2FsJb97Gh9eIxgoS4JyF1u8k3nlLnj%2FpZhg8toIFrEoKe2v5hrMJJ6Ar4ZJVaQKjC1o5bJBjqkAckLsC2OCbqZnez0K1ozgaUVH3Mjjtx1nQD1BnYGHF6IB4zfYh9RtCIcR4y4ZAYPlPzBnso4i3025uRx9X7BAwgBJWhJmI9MGm1EgCC6tHTqke58ilMGqXwx7pG5UkCvnCmfpfShI18IvNiOH2vabrxSRWyJ6npsgOZ%2FNV3iK33aXg2uT%2F1TXQ7bUG%2ByvS%2FhEwIQxRY3yBJb%2FkjdGN%2FnjuRAmJom&X-Amz-Signature=a2738aa094dd0a83220620e2aef7a3c99190bbde73db78f2f23e94c54db3c744&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

