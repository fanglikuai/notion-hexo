---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2V7VSSI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T040212Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJGMEQCICJLiI7BMkbk9zQaDVHkexC74GlR2CxOA2B6j450H0ABAiA1yRrw3gA3ZpK9Jc1DYWf2E5TSQqswmqN6xuEOT2DljyqIBAjN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv9JrUjymcu8lABx%2FKtwDmJIeZix%2BYKPTs3H%2BQ25vYLXqxVbSXlEnM%2F2t0ld6fjAfr4KKPMK4J7uXGAcG%2BXHEcCLkvtpE7%2FwUGkImaft1ykGEzOVSNStbyf%2Bkzt4ZZzszlKSm9gT8trE8iPFw1ZltXtNHJH1geVvcxMQJtBgTDB7aOinQuFvL6D4L%2F%2BDis0npQkYkPUeDJP%2BmPkpWvFxlVnIb%2FfRO6K2%2F3c8%2Fl46VTeORRIZb2%2FoFsGjVJq96a5dzbtexHuVJqaMwKd89ZjMkimDKKOcH1fZTLsDVXvw3WuwBT4Y0EIEPRiUcvKIpz%2BLhDa2rpVvTX%2Fki%2FOVjNG6IQAbW5rb8rW1apTYTf51lm1rinodfNBeqMbnmmtgQUrEn0O3uAcv4cV2P4apWizhwHUoF%2BM1Totqe5tqpiKKAFk49s19pxxu7UeRic3hfEAhtprlUdEMXVxxJaPl%2F3m8xE3uhv%2Fbp0xE5JYY5B5ki%2F0mS15Gd%2BsbQNBQMqyxEoyF04knaOLVbeMZBn00AQw53oHjPVitEp5vYqXkCTtKBWQB6XnSr6pG51wZzwkvxcAlnCSE5rDmt82mKXjXCzL6XQvXo2UUZkU88q9eXYaqUbJKD%2B2rxr81zBcr3NBZJx4EoQuptsLazq3Qd2xwwifS6yAY6pgEHCF%2F4QoMXuYzP2EOp%2BBgpmTRwH7AOHvYL%2Bgo3ki19LgiMdv2sP4Pw72f8Yj7LNp7tW2u4RjsSYfOf4DQTwxNnY1%2B%2FAHqERIUeZ5yc0IFOh%2BUlugApQJsPy%2B5C%2Fuj8UiULrETdGgWNZR%2FvjSDjylzqwPULEuafSaajGkPJdi68QY2hu6DP%2BegSjS2bknkuJ211r8sThSmqTp2Ey1NWJ8Dxff6Z%2BIGJ&X-Amz-Signature=4283bc17c19788bfe61a1be246fa82e0defb14ef5137c4787333dce8f70b3316&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

