---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46647J47XWG%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbbByXY5qQ0Yp4YTYCMzDXJSzQeGTCYQOKzlh7fvryMQIgJqUEl8ix7gWxwXudqofh00UT0sp4QW21L2q9nBuuJJoqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKBvEQHXv5KtJ6DO2ircA3Id7ybQRIm52x8xJDMvhimMf0dwrIj3eErKe9KYWW8p2UVLtdXV2E6BIqO0O2sFNHDIyfYGwUJWV2NsxbIY%2FpJZ%2BJ%2FMoK5rxLDmFmY64oVgfs8pdUmdDLKUjSw1Rhc58oJOE81FtUoW2FZWg9iVWV8VWv5c3HhTYSWI28jD5%2BKba9X8hkb%2B0s6NlUWC%2BbDifuK9k%2FtJAqMINZBXN7qbXTkKW365a9jhBRuvG1pzyemVRkryqXw8U9KXiFpsdTOnkThliDcHPlkGRx0C1ey8hMQNBZvOhA23npDTYDYzIEv75PJyMdfXYpnDSwMYt8EmOsdxhgVfw83uj%2Fe5jhSMhjBAeUqSUro6RmXRZW5NL34qo32tZX5UB5Q8C5APEBcE5c%2FDsAIndXyenitTayn4pMW1nYnrQMpsR0hiW%2FUD2GsrnyXJX0XtSZLWeItNqDzJj49mai03cmLkgXhbL20upBfImMulOW%2FRu1PrqiZ05Qv26GFanbdzTWTBTvLBadAeImKZMkcVQ6TLHOG2hD16lkxZpJP2jI3V5axDNQ5zHtl1yw%2BEUD5hHMYy5wWRGuknBmWZ9y1fcuPZegC0SIZJ%2FBR8cYwC1hsK%2B%2BvjtVHtLcccIDkY9dJ6KqICSomUMIeNm8kGOqUBieLnblKx8pFNWgz4uxo9DNq8ISEFF1Pv9gyFt8ZRjk0cCEkhoB7Dx5jPgNQSprpC1ag2nY0iPyb5TPOGnKLgS1dch%2BRaIJWoPbVv2KQ0R9KB%2B9Xetke4N0YHNAjhRSl%2BUJYWF0xhZFS2xXyFYVhhUJwp3OdyN%2BqyFaD4Fkqg7%2Fy%2BvK1KgyxoFR3scRQOwHp5hc22miKo3ZmSw3%2FjpT7EcN7VkE6c&X-Amz-Signature=2fea7fe4b129e498b3136fdff59b2c8950cf4df46fc263b3d31bcaa6cc72372f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

