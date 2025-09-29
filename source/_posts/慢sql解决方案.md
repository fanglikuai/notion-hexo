---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH5ROVTL%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCICSgz8mylNQv%2BKA0rzRg2x3UCqGkz2R9Zq%2BCZVhL04wRAiEAn%2FOy8lNibW8UkQ1zO6zmsGCRhhbMd6hi5bbXS6gMF90qiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGngDjpED1SK3konYyrcA65J4lO70%2Ftk33wXq1zR0%2BWJLTMyb0RAInNfNnaEf576hXjFb2MTtL0q%2B66SBMZIVXC%2B%2FeNlQhd%2FQButZUxSoZPxmHqXbtGDOG3jPzQP7dTLsuHJEp8Ht%2FHNVSX9rht4zgBH7diixfA%2FqXREBXftE7FvnO%2BM%2FxgobN6GtumMQfr46Gdlvxp9xVGaOatRXYxXArK1K7cUvq%2BuXyZqSFgKbqcmoh04kPtDFOwkGmKSHdneY27pCBr8VAvvg8FfznUZVp4FqhAEIXAVpmWa5pkbdNY6rpptUhOYxaicrxa3daMROspXQJ6ukLtUmgumnlvWB5gQVUXSdpeawfLxvhf75LRM%2BRen6MhywmocImj3L2RBbWrtwP23FAmORQ6rOdWiy3wgoSf%2Bt9pNepsA2vDq3DgqT098ADHLxqomEvbXr9V6s9ZOkmtT2lzaN45Wvwhd%2BndIdU%2FGJDI5mn5rVCHAai%2B%2BG8Sl%2BNCm%2FXtEF4LxFnFcuynoLHclJB3R%2FJuqmZwT0qYOZM5WXKVxx03QnCoF4mFInNJ3aNmv7MjSX0%2B6jBWiJscMq1L1BUrAHWfcRGBw7%2FTDEYeTNUNSPVcTbt6xHn2AvuXRrxY%2FxuXZC7mUXO2us05i1q4Yv3%2BQT1D8MJfX6cYGOqUBmJKCtOETd9CjqJIyQ1sMM4CAOU54ZU5Fy%2FFkwEhq8r1i0dIk%2BWZcK1mhCWaog%2BZ49xf%2BA7ZeN%2BR3tgahNL1hV09Nn%2FZoo%2BJRt%2Fi14XZ1yUcnNq3sfRnRG%2F%2BMxD0vB44x1QGQicImlZwD04pUiTy739CKfpg7FptYWlK%2FHgd9g%2B3cN1E22BIlU2QfcPb02K1JA66y%2BHjybR46HaHeVG1VNwhNH21x&X-Amz-Signature=28312b1b6bc08f52bd3f6d825a510545cabc0059f1e4d504c1a2b058b592c127&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

