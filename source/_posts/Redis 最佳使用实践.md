---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QLCP4XX6%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T082105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYYZAgpLbF0KnydhJFzoqm7C%2FpzDZZgx3ImYbRk2xCuAiEA3rZcoPCAzbu%2BzTnDM28v%2FuyV6OmMswZ%2BmYfUxVXAIoEq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDCxJOJNB15rAcHKaMCrcAxJ5%2FS7cg%2Fu2U5iuHlTwW7U4zrRLFKhlsclI0Qu7bW1E83HM%2FvycNsNSxcXbixvf69NVHe77bucmnyjAIJUX2pv%2BMtr6I%2BR%2BlcG1T5Vt1xsLwZrxuMnuwG%2B8DLDPOQJlZMbV%2FrxWgyPlTMG7JO4eGO16JYusOjLQpjR0mOFJ5BXNIs0CZDaWIViQa7KHHq3tBCtWjr2Z%2B7lhR0IDRK0adK1aJrquz%2BnYT136eOtJbM0T9%2Fq8u4nhPldOjh03F2J%2Fd9NnjAw7A%2BrsAfl2Ne1BhK4BFqQ1hHgyHGMBcz%2BWm%2Fh9NUH9NR%2B648p3uAbsoJFTMU81uAWpl9s98lPf3Rvd3FQup6TevbpZsanBDzOk99WZKOysHvUW%2Frj3dMY%2BByunDi9AW2Pdlrl59ZWyD2%2F8O%2BV1xR8nQSVRr%2B1XSfZB1W%2FBv6tShvsLdZv5dF7ynzcLuTP%2FnkJ7eXK85xFcU7o%2Bf%2BFxAUO2uDtEuuKdK5lDhFoLWdlPOhof7tMKjHAdofWlmLfpYJbmGtqOfny26wLHbVVy4MFgncL36VmYlOf2JChG8DmS9j64%2FS%2BOhpAD%2FlOitQz55ZTx238hMWMjIlogK4MNx%2BNyghYAFHYj6ZHF7jlDZwDmB80yJjy7MQZwMJGTn8YGOqUBIopqxtIn0XQtcWIUImmRb3zqd0nQXqwdTgKTSv0v1IccHK8KN5ZKixgTqGD2N8%2FLK2%2BupJEdNS%2BriScko1YFQVTgHqs%2F0gsuNDpbGBkgFgLtW7giYM448kXv8sZNFJD%2F05XcMaa%2B%2FFRuzxvbtzftlCVTrDE3caOgdmin0nuUntaQNAXQ3Sd4DtmMl6ORWuE0NbStFraMnW1rb%2F5IucVcK72VadEU&X-Amz-Signature=72eb03583a941439bb1f1d9768fd971e73483970fe5cac3c532749e3c8d4b010&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
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
