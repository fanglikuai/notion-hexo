---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AOJI7ZG%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIQDa9Nn6eBaAJ2QLCf%2FdN9z9J2m3EK7KLvaTm44tsAySdwIgPrilRDvl3OLngSOfw5%2BCQPSJ4qSLQwDTlagsB2tGtuUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPekwvHSJPisrbILIircA8KEl%2FuWZpFdrz%2BVAmIIJeP6%2FRBnVjyrWMckQVDIn1nBTUQIYCxad5Urf1Lg6rQgnk2mEXPAjePNwIm8qo7PIQAgwls5tAuW%2F6mst%2BFJ9qY0HOpe8rRilVvaTu2nQXL9QoyIt2iK9ETIG3nnZ0vXaUAp2cC9tfd6RqOYJsHO8il5ZmMaBsu%2F8u4yckraw30byXAX4wozEm5JjJZp6zeNlPa2RAOAr9hlRIxZ9g33rBXMm0gyBv9rMNeA9sM9Bm3L%2B6NNYs5tJUAHXqE6RAwTPl5WQRvcfWXp8pziprJ9jWZfG9fxXHdlcMAvVp8jTBUQ9iKu3sfrUIVPq9AyHO6sNYvmwLihY%2F1cKgQ2cuDESzi5%2BVvhbED6o5Fp9whH3rItk9%2Bo9dv665BzO%2FUdKUwMtW4yzlqekgEfNVI2CHH31XAXUdu%2FtfF519i%2BFIqafgT3SS%2F92d9U9llB1adP2KHXOTo4oJb2sScnQ57t22s7s3t81bY7HlIQy59GheTI5LIP%2FQtNZeFkoqBn55kCmhC33%2FeH5S4mRIwxgXI%2BQ6wWatF2VRGA%2FGTOnizUNkeRxp58f3ns8rG28Eldu7RFDsdJJUYGaMxZF%2FS%2FwY%2BL%2BuNjYf5VGrd8juX2cJuPuyNXMM7U6sYGOqUBZjTN5DAo%2F4S49nCD2LKjvcW7l1yBtxD%2FN4mtOtj0s%2BgPG8FdsTyWiYReHw%2Be4CywwwOVrItgBPXMrukZw2u1DOvO4BsbPtjoHPB2rx%2BCcPQwe8q9x3Jvici14aKHg76jn3K5X%2BQFG5hDaUNnLpBOXF1IuwGaaNl82OTV4Nq%2Bxvy8L5Bqxo59hQKrMqJjGDBb%2B4tW%2FEYwqNeipuJJ4TtcshFwBlZA&X-Amz-Signature=b70fe3d0f6ecf051362cf9eb7c200053ad1e9a2f36367cf66fcbe89cd0e954be&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

