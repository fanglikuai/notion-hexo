---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TM2EOCV3%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDXrBBN19h%2Fg1U6SNpOA%2B0%2F6i1VZAzrbdZ2fYvhq1ahngIgD8w0Wlll9YQijW%2BURp1S4eV6lILKU7Bg4UhhdZfV770q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDFRkm0CIRvXN0OhIvircA6hyNn92wizzhK4WfpzNo4ax5UeIuUtI15ZXEOZIRWc4%2BUgO%2FNWZI%2BBGvDMWgqIUFgHDxg2SPGLekklovoDO2ABKR0uPscapulPGvCoQtEEL0nodD0nvJESRwD8FSW%2BiB1WNBt%2FPz0klcaEOfcrsOBnULrYnwiJ2cra1LZ4TnIy%2FS3u9PTCTCFud%2FyfeQVsXgcnOIM0AMyVs1WC3GHFgEQnKOCk59xZzOc5MuSnjd1e2xU9QVApKcr9j3dmTaNDGkSPZSWr9NwyZZR3BxlVjtZhwLt8OISOBi2ui0bCYQKXlNtvmcVyVdy1JiqVSs0%2Fr3B1F6zfkRFp%2FhFewos1kOJR4%2F0gNZCFETcl%2BYELataxBtRfw2DxmD7uCVv2EUpYIuSQKYT8%2Fzf8bzKeF1aadhJElvdOAh%2FOYLiZhUuXItUPmVBJuhso%2BUYdSHDJgTyBGRJR0nw4RxlM9ffRY93B6EWIFeIFDLO0hVQhq4JtfdymJDxYcv1IhO0eyD70%2B%2B7mPZwKl4IfWnUgW3Dg40uWYdgrnpBgIuCVb3%2BypCJQA82%2FTQWFK9onMoJ2kp%2Figx%2FOQPeyMLtWTOBc%2BOfjZaUfwu2ynA5NYa9gN1GoTcFx%2FwcnoouGdYKwTWcijYm7GMLyzz8YGOqUBRJfeJedOXpHp1puwIYAzRmZWETl5DrdCn%2BXbpkDgVqkB7nQeL%2F2uzVnK%2FEgPUzo%2FFoguKbZ%2B0cUpr4VplCNHcxC%2BVd9ogzyKiTwXj70zsaQZVV4AVC%2FAVaADhzPbZ7lZbuOAJEe7xf3eSRd%2F7dQ7FWOfT1cu2jUvnIsY70Xgn85neUXojrCE%2BOrdwaCOT3OapmyyfO1RwlRYtpWqeKMDvMPOLRDD&X-Amz-Signature=bbee5a24b23517c5754423a33b192b09ae32e4614e404c94b1ee459f98b5f968&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

