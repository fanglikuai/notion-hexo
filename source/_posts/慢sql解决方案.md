---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZK3KKCKI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDhogvDb9c4JZpvfC%2BO%2BxcjYldhnV5EJ%2BoD%2FL7H5otzxgIgTWPHazrHH9oGKluUzUVXVl7JERubBg8JW4pbDr9ZeiUqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFlK5dF3bnE2OGbEOCrcAykwmBQSRvUXHm%2FVlY2c7%2BxJTOeQu06XeWiNRV1x2q1PwYv7ba4CeuZK%2FBPt1Kn%2BaIczz3x4twnE%2FMSl%2Byb4wSlXDfqoW%2FnG9b4udDwXdf5L4qD90Dw05IltuG2xcFSwzjJRNWnL6ZAsRznt%2Bgj82PDuZhtj%2Fby8lVuDa7VJR%2FbpTp%2F3rODmBv6H6WYCuChU1UGiu3RCzAdIKYVrpgLDnS9LRWvhX0JgQb1KUYOwtUMCDRy7p0Yo6152B6V9FEWAPzdTAmmGrcZbNu5BDxnoAGXF%2F7joW7iB9uPtx%2BaUJ0NBsvbUVMiNpNetUwnFuMwMklakOllhOq3dtUCwaExeXCGtmzzlOMzFHxNaUo8L%2Ffiu0c%2FgTvWPkrE6OMyefRIKGjKQZAFxVeqPg0xrYcUHGL91MbUOn16S1KSmn36%2B1rNa6CzvhIskoYo7aG%2F%2FUjp6%2Bnu5P4a7%2FQACoMEj%2BUoVoFr7LHWDiZ2FcLYcGNWPI5COr%2BixF2O9Ol1334lzl%2FQaai0vI48qSFgHoYyHcx1JJF%2BglgjnANzwlmBQp0lrxIT8XZ2Fdv%2BAzqX%2BRKlq9IMC1k7CEh4rA2GdnudtVrDvc2LCgy1KCy8islXedulj%2FYIFbPHkKYm3ViMUvvzlMKmOvMgGOqUB1eY8LwqojtUuBlf6vgFoZvPU6n8Sukb6cl4491dZU6qCICxgvEyAemzkCuow6vlC%2FJBagEVkgy7TSKdGRKJ1QgiLjqi5rKxZ7S24RZXRP21uubsWq39Kz6Bjum%2BbP8FKR7yS0YsAffmqEJa6uyak8Ni2iyDKtbVJD05gOJs6k9orfj3QhJEXV8xDisEgmXOgZEcV%2BJ799qzRv4Ar7RuNCEzYxQ02&X-Amz-Signature=eda7ea8878810966962522518702901f7aaaf9ff4195af444f7361c35cf39f0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

