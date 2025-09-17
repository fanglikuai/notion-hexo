---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZOOCMZRG%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIB%2B%2BIgV24kYAWUGqnOKKlogyHRQm1q9sfTzjcImI5%2FZ3AiEA%2B6UKZmjtl96lIAvEFTAbrI5VCtmj532LJ1ebMK1X4qcqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHpRzLGU5fJrxUybnircA1Mhz%2F7plEiEpjP35YiWG9NkpI%2Ba8HVBvw8MzsfhqZzf18hDB1f7ak1eaeaFGpdDlQjuHSdmxw6RBCYboxtE4c2yaJ%2BAyRQ0diTbe1D9gT1JplQ3VFeIZFxGs0NkLckMetdwhC1hArljkGWyi4mogWezmy%2FGFdV3nLoeXou3fj%2BKGMpDdZBCh20sJOY3zYlVTjqqUcfyerh1l2IkBVDt%2F%2FMUc96rP1inXpMjb1%2BPwnef7tq4nnvIvMFgC9%2Br8RJYoQW9pw%2FSbCy%2F8jgihdTlRq99nqDJ4JwcBRpIxulNiKvgZ%2FvGUHOVQTDvN3pf51m4k2StOFFk7J25bDRoIdZqgsFVjyDagw7gx4CQDxpQ5TOjmwVTCXmY8FEZS6kh8hGPWj0JOnoWDlmidQ3IOrLmLH3noeCYSI%2Ff5gWHKSQkGUMnrrVNQtbBfjn8jq2UM54MU7Cpjl9dGuDMHueI5%2F3hLMJ%2FT51pGnxM%2Fcgcyv3ysiKJgVdvlA093YCXUem86M7xAcCWRBxThXHbYjMD%2Byt%2BlEU9B6z2joDbi2BAPiK68n0GLX2qrDLvE2urk0ZG5lYZ%2BWhgwLQVIRBmRkkrE0ufKZC1HfLMKfGfQLeuLHVUXeeAD44JcJ6Wak5YdBabMInVq8YGOqUBwarwJ8%2BgW0%2FH3sFBcRQO1cUMPKVqLRaYIcBq9tiYQ%2BL24LJpQFd3s4TpUN%2FEYPabzxiZodRAIVoQciSjinzhlYwJFdawVoWSPDu%2BlqBfUvn1deJPBkmDWqYtC7x2lkffc2ppKP3H%2Byzymrri6mlMG5tzHZea53Z5YiNlLjrioGjY7i8AngEuUX8gZKnD32j96jLAntQbb5Ml8x64gJ4gRhk0OKmV&X-Amz-Signature=badcc41f13c9e2f451c50221a905f3bf76a2ca68193dd4b93d13dfaefa8dc92b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

