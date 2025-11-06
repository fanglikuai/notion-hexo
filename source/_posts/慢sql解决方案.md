---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI4W3J7B%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGm6Md5ydTwUVENuzCPmtsG6jmY6ZQtyPanX2A3SVngIhANrXuQBLS0EZh5Polh9%2FfURw4vi1wpYQFTMGWpPT7MFPKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyTtY3e0LE1ydwvpKsq3ANv%2FlUFYRgk5RHcuP%2F0aUzXtijUo6elvgEnNugllBaTW%2FgF20bPrPA30xON3fR76iLYoXDeG6LMwJGSVLrDlBmuIkmmPHOeKWmjjja5HCcyPTrTfGCWkdpSSywsyrLxztWDgzpo%2BmvJZxb28SHTGp8tEP8CgkJtscjSlndyek%2FYKeAmxHT6rop%2Fues4G6NaAct13ubmrb9isKMVUPA560an5QJ8%2BH%2BWJUsptVAo9D5CbX24EoGeq2smY2jCkmhdYkTrsaaAm3kvx1A5RI%2FAS1dj26GrEWJX1J26mmEpcR2AqvjSyvlUdhubzWnveTnyGfyuOOZ8FPXu%2Bq0soDk8KXE0yYmrWCm3StNeBhGdlUgOHL%2FKIc1NLrKe4t9kFXrd1UXrqX%2FO3I6zpiXDjtj5Brfoo3%2FKtaZpAaisXtQ%2B53SyjoumxBWHqEEF%2BdiasciVHnAmjayS0mJoNnQ53O0aTDehKb65FPDnM%2FYm1jXEHJD2UOv9jPR5j2zB%2BLv9TLF4gl%2B7%2Byp95q%2FO0hDP50iwBfr9l07XJgJ7pql%2FxwEKVaX50BGIdwvNWznrkyTFC8LTFykcZDSU7m4Hk%2BmKGzvwWe5pJiOW%2FKgpFWtZUsjwSr5iLOg2ERyRd0bdArsvGzC5urDIBjqkAfuoMGN%2BH4tpVp5%2BgMoBYEmFpIuWnj1Zqf8fZzxJ5u%2Ba1w599jS9ozRrvLfquBfycBtCCks1iTMosmZ2Nhvn8mQv4PwsoIwnNwn%2BfBbUpSB26AFZKktlhbDPqNyRlb4vqlMFvLAfbkapWtxLp0%2Fd4mB8WZ4mj0HGG%2F7Ilg5D4AR9pJy0Yg6kANXPnyzr0ZvZ9P60NLdKco%2BeSOjKHZFiB4hzHoCx&X-Amz-Signature=2e779d0d07969f03f458ec36fadef1a4b3d8238a102b02a9dca4647bf622eeaf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

