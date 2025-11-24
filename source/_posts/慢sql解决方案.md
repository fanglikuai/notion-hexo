---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OH5X4KM%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T210054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICSOnr8dchXqynrdLKafOZYh%2FQiHadANCuYTFaWR8iLKAiEA%2BTwAQb9V8A%2FvR93KdAlLr6LvJhjxi4PqVYf5iZWbuSYq%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDIJMMQu%2Bsqf6KiNpASrcA0uUNFBGgOVR28KyXKz5WHscKbp2%2B8kWzPXrRZtQuo%2BHPdsKDsgdPH1vnYQ7vJye8fNaMFh%2BcSegwOcr8Q7G%2FL7Kry3HN8LFfzBsS%2FKYPN0jxmV6iAxEt1sJfl%2FCG3hcxnWhqMpIY1ANmCmjXUUtvBmhfLVfZNyAIMJ6yGzjrZ%2FDOMCjQ5xuRRdsP7zMCybmlc19zwSUa7KWRCGlLFWgoMiaDJK3SZvlzW1Nn0Vl1qAvGr5cxtrFdGnP6TcntEVDxWhVBNqPx1zcCosfiwURm%2BtCJdWbOkfuSYneL4KHwTrFXLL3cjezS1OUEzljhnNP9AvdxMajbqZceVJc9%2BSjoCEm2JBcJ3wdgNV36QClWBmk7jFvAnMp6ZDQfubRYacunIKXxN3gAbv4J5cwSi0GuKJVAkGt3Kr%2F%2BS4xfpCApV46BO%2BblpMRGUhWE3%2FZxVCNHz5qVA5ITnja2c8iQL2%2BVUrBKZJ2pLUNfTBJQvPeHsdrEchMB%2BVsrxrWMA1JdjvsDN0Lzr%2BREIdlYILtPDlXMG6uSGRjhSHFZ4xxRxnG3OgeJDBUHB5ts5AwywJI2F7Hn4wWxrfu0HVbr8Ee4PBeA6osndj7JUrJu230CpstACdOqtSnQcvJRwygweuJMLH2kskGOqUBOOwN5y%2FNag%2FENjuuLwex%2BCeAQG2X0PhoIxT0vtU2Bu39ZzErjTOSgMvaD75pnSKl428H8xkBY%2FXo1m9sj%2F6HJOBUOVwo0UflXMxS8aUtNmcL%2FMlUBzS1YT6g58TrJ0fhlesZrq%2Fv2dd3HPsvK%2FCqYe%2FFPfhpDlGSA3VG8ftxMfMoHLoan%2FZgtTA3tcfcHLNXt2uYFVrAGNsq3LhBi%2FlOihriWr2K&X-Amz-Signature=8ff376325c415e65a31235607b3b21245936ae51362a579eac7011f884ba638b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

