---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656JPYED4%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T160038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIG8wQLORvcPHJ4a8V8kHUFKGo0H4o0BjEAlW%2FA%2F6J5dPAiEArTSw%2F%2FjImlGfifxi53D68GYJ0q1XJO3NzR07Zg5N%2FJcqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM98q93%2BUgevQ5ELpSrcA%2FpBTK1PJ6zoo9ybnnyL7jT5qwyNDKDAnEKVMLeVPOLwgYnjy1WBpLg%2BwKmpFPBj%2ByTd25OE6RtCtgRS0E7%2FPpyRWvB%2BFHhwL%2Bq8uhZGS%2FPR1oysEHoagPWnail33U56Jswx7NfDdKIUDuvoDvt%2BLQm1AuvYlfZNyocmglV0nsIyvxkEtGpg8jsia0T8u%2BsHwhisQH18t7k8Ai49BOdVni3J7Lgima3LXK4Oyrme2qKUFVUiIfusjG4eE0akJ2vDsGAsh0LAraEloWRUbhSc2iT%2FWaKmTLye48PHyqbl38AXIIDK4LFTzkCaz%2BiXn7JEPucaQy8qvV7JsgfVuzY%2Fz4g9ZkguD5B8RdkS73lB1YUDzFgPRlS8Wj2OxCTDd4okvhv6%2FXrWHDmC2Jmij4zF%2BcfcoCno%2BQN6xDH8YdNRla8d6kz5ajkvwz5i%2BU6uiQoH5Bm2YUyBZ9iBxRkrLkRvuHlJpGDTQD%2FZRza0A5FDXbInQs1Q9gKT1gnrM%2BtaO2VeUF5V4SHx23sG9W6dAY%2BbS%2F5mzsYKkx8ZTn2bgYMW1VlSyfNkH9n3HqsxOWF7L%2BBAyBEDIQ4txlxQ%2FM1rg%2FUOBKuRlQn9TI7rAYoIhfObbfXZTdDXd6zDyS%2BSoCY0MN%2BKzscGOqUBqRog8Xb8rQ9M3Wk5PqgwR0UDE0%2BDrFT%2FjSzT4msMsmVo962qKypTkgbs%2BDIJp6uZNIRadn8b4hMaKs0pFaHt34db0ufMpSO%2FSVNYeRNwKtoQ0ep7%2BZSd0BFZpre5tnWtY6ou27xzPMVBTlMVXg%2Fhh%2BPHSOPEGVQHcGHWeZ4woNTVpMXItxlEi09n8VfejJiWFpgTQnEYPrzJqOMqUf0oVHg%2FJ822&X-Amz-Signature=490733f2149011e9ce84030450772a85cfe817f61baef3bc319729d0ecb2225d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

