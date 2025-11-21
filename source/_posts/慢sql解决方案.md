---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46662IMYL2T%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJIMEYCIQCudVeQMEXBTwnivbZVonRKs5YO2EfqAlaI4cf9ATJPUwIhAMi8%2Fdt3eO6%2Bqy0VyDQnm39cIX57n7qRR0X%2FsdVwHKPBKv8DCBIQABoMNjM3NDIzMTgzODA1IgxmlBs8rieNMFcSVmUq3AO8dXbmc220AFgTUmdc5GykY1MtCrjo1p0Q0FEtOnJ22HkXiiBt9MQGrV%2FFt9%2BHrRCuVEaUw7af0tYOrgNlOsT%2Famc4fe8pq6%2FKr0n2yA0kJAoYN7%2BMiNFbd99umLag9bIkbYc%2FxHBNqSm%2FxjVf846s8JsX4j%2BRaozBK%2BWgsxl2gViMNVNGlKqnnsCPSM9FlwVXzejYe%2BnrlYBzWJB%2FY8jkyyUOOKpgUtw8Z9T%2BMs2Mkj8F7f%2FRUtyK52TKTGhgTwWEzr427CnZsgs6k6jFODHjMMmPpKQdJYJA%2BmCiS%2B7mvPXpe9epLrR2ihIMMzimBA9eJZA8eWInUfpUen9tgwimw2jFbwllXj9HYFpeKaXCGamCvDL8nZTYhfAY5%2FUcow7hXukpo93SIxMvosjPLwCU9UN35a8qthyQKKnwuTeJepHWJgyeqgI23OagXFj3qkEpaydRj%2BaahE7J8N%2FZpF7w95WJ3Gm0HapvmAtzjtYqBN6i12WhpttpnXuDzED%2Ft8FtYuDUo5hY%2BKjcmS%2FGs%2BjJX%2FpC6Q%2Fu3xOW%2Fy0pIFcG9yoQCMXC5GaIzRCzzK6td9d1NhKUAIszEpmEMP3xNnWq8yv1npcZXAfnpfqqrGGPF5qJxZkfbh%2BWDo8WGTCtrILJBjqkAeNPIBb1tGKZD8H23%2B%2BQtcRGAibBSnMVaQXjbmHnFQ2KJRpxtktpXSSm2M7ewf7oXzOFphGtmhd6dtng%2BOQakVI074WIyJDwCPHVh3bgeAOfm8TDsbV2j36UiSezqGmYPq%2FNd0bZFSo0QnBw5xNv9qT6KyVprhIgi53d01ko%2FuZf9UEpGyfOF3MdJ%2Fp0mraUPSopZ7OYIwfI%2FlZeR2tG%2FwXWXyJ0&X-Amz-Signature=21c917d5c5bcc2b7742601da206fffe1a331ca5410c295b26503cbf4dcf83cc4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

