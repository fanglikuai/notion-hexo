---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q5HKTAX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCpPBmyJKvH%2FSjmTiiaXmY6Ql5ShJgKRHNf%2FCMPQtTmmwIgXhUN%2B8cG%2BkKe4WirOAPLBfE0FijupOCqfWMqd1pzr78q%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDPerhspGwOlkgIP80SrcA9THSYyUbr3ygsvnkoffblp3AzXS86vBiBXdDamTU0nIsbAC2EEglJlfjD6kAWjhW49HeZ7Wd2D2iroN1iUbGlks4awfDCoFDX4ckjHmnHSWyBG2TkynsnLMqLGodYoMvHYSWYFgyZ%2FHXEiPCxuqEgA62xBhZQ%2Frq8DWxHKM2Ml%2FhjNRzwmpkDDk9trKUu4NryUz%2F3Tf%2Bv4WvF5YOGVhOc6n6RMgoW%2FAUA5cmuwj22m2XHdyIH5fPPHAuz17dgooT44h1Xc8Vvy%2BkDxxOJqBpC99ZUGk6BjdXpiPE%2FB8PmVrZ6uw2qbIRQbVKtLYuXHLcYSKLgZdq8%2FAMkH5%2Bv2EvcEabv3uFU485IRdCLHTvsoF%2BNGvIgeIJ%2B7IOs%2BqbPD93vfW0LX2IxXz0uKwvzr1nhBTFtpPy5an8J4OYP2irnXIGEqkWTuO3HnIYY5JGgA9nIlcK6NVUQSUHLjmSaKwCLIup3alUsetj77nJT1r3u8nIgFHu48OCUaCsav3DrR5PVVZDgN%2FPmiwkYlCpwTVdNOIt%2FsYwXykegQI%2BJroS7aIQBTXujXsMg8%2B3HCsDGJvSmRDao4%2Bqs6Gd30ARy5EC7DAdpc2O2jmLORdwFo0zqFiTAq1wqfZZCr4EyPLMISZkskGOqUBHZBiBM48qEczWN%2Fol47EmpJiSD0KvzEd53sVySOKJM51CHkegT%2BfV24pgAFrYQhiwwrZ27gumeV9KpNvFn%2Bg2J%2BsA34aBx9q0JFZNsxGifCTZTWs4PK0pUxxcW8rmvlqsgzXEQFPjlfYaZMN%2FetPcSyfCDlBRY03E1JevVCbTPAhaMFIpwzsFwuFHkuRCOn%2B41LBTeGA9xxn%2FHcpQh6qSLJZM90r&X-Amz-Signature=d5c885f07e4d8bd8a62303c69d709ed1f1c4459287efe8e47c392d76bc4afe76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

