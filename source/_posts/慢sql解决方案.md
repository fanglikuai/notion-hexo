---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UGIZUSA%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCICHnfpqI21EgD3PQfh6FkSVXEmqfA0rPlveOFeSDlTbZAiAmBCOeEw%2FB5sWJEhYDybh8rYTtJHMzCfu%2F3%2BOa7dOqHiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAREGrXuvHIQMmoA5KtwDov87Sa3N4H7TBFoTuh8o6UX%2B4Pfro2JhansCFaIuLVms%2F4gSmOg00YP2Fj%2BEE2vDI%2FGetwFTpM8jf8eMCgPB683%2BthMnwaLkWCsNnFyNfp%2FR5tYzpFCF11Hmu7NQsu0CXjYP%2BIIV9%2Fv3ZRrb5ZGKmeWCjjMfqbosTR8VtSNmdnNRfSVOtFLI7WLFpYhITti%2Bm78ipkVcXxfEoDLNHMur%2B3%2B7uMVVwv4OrZYBRLFcwGWsYxx3EJNK%2BBzhZQkn3LuA9ZZ5LGE1gnCCFTLNlUuoQvYNGEIuhBtCueKWX1dMCxkHeI3rwOwjNpHOzi4v5%2FMMmoNEWcfEtNAbFfqCoH9Q76YSdesND0aKuj2uHFpQxdbyMcWJcQ65iKYJtaa18KFxYuYv%2B53rLV45b0WOOe2iqpXj%2FhmwE91RaPKZYHrmbyqoVEIbY%2BkuV4B2znp4fstOgzwEjtea5o8qi8J%2FxQt%2B1VHg%2FqOXjFWwnJRgDuGvmRxSRjEU5umYNrhXFjvX2cczv4bw4ebHo83x4J63HF4AEMAn1%2BDmdzMOM4hJax4PqMCBGMAadNf59PiUVZrIWE%2BHL7EYQvwDDnNpxQUX4P9pn%2FeKf7gf2y9n5wbYwOM3eXKKaTw%2BACE7kjxOxXwwi8y6xgY6pgGm0LbHrltlEdfQv3TsDFh1NliCSAxLh%2Fdwf9EAVdnmGtOuILDA%2Fn10xCiBpf8CClPctHB314lqGclXz88yR0Aa%2BkW%2BMlks88HzWROgrv2tUn0Le6xWFC68XBq0UdUDdB2whr2aVUs260ptkxMTQMGfkcaPLhaynAewvOfM%2Bwg2KT4trHWjkPY7bD6j%2F6eyr2cAyauykNBfvFbkiKcR5Ssx2uRVcfwO&X-Amz-Signature=28f5905d8d218f523e7f940e843902b17295629e4da5dc829b57ad7b790201a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

