---
categories: 整理输出
tags: []
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWRIDJKV%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T124211Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjBby%2FuaH6NDyoJgfQ5ddtKdyt3Hpr5xbqDGdc7ITFJQIhANQEF299BXk%2Ft73%2FlqcVMklsC2opt7tDI7JnUAKCsFZGKv8DCFgQABoMNjM3NDIzMTgzODA1IgzzcHCAK3d3ivRCHKsq3AMLDWWd7bdBMrY6iTLAU0WKqgVAOSW43jF3Vvuu8T0SIsRd6FfEoNgdWsOKZ5hqMAIJvUEzQyur01MLTFgNsvbK3gDGOl3SjBQc2lwxa5zeqUKAmbJzJP0p0S0FIKJH57L9Zu3GAEQvBo%2BRXnWNMCy9YUlDNj5sHseSZI6bsCCyzrNyVv5Nc4Yep6rEBfJyg71NzZQysBgK4GQyoxgNujhahrSYh96PwvJDmaXeala0uL1kDTU3ZKcwza41P0OLPGdKMamcwYOiqxi76tXJWSBS98cmvR80yGm8DsPylUtdcV4P9PSherRLjonHpc0S65V8HsnzyBBSjaHw2UaoSrVN7lEe2V4BDZWD%2BARuyb7SPmLaIE4RK2sOOr5UWS%2FKg%2BtT%2FNAXvSyJcQrBOgQ%2Fl2fnFqBkwlQuoFcWmjAgeyra6TuMAiYmLkmEWcv%2B7OBF3KSCRvkYnIx7jVyIWwOglzDU0cdz%2F%2Fv%2F6c1cnwu9XoEUNrceyXKzRXw4YK3hbMdZ7OPIliXDCdf68PU7NCPD5ht4q847dmSje334RBSLjXZ5wR7Gk23%2FGibCF%2BmVy9BhQ0%2FjrOYb7cRNBQeQEBe3lf9rFVqD%2Bb1TbKh5yr8fLnjwywzZUOqgVrFCzMtPMzCdz5nGBjqkAS2EM42ISP65t%2BKhEHNfxau3Oom%2BZQFBvaeeqEyydehJ4d5QV8kVPhPCqX5hz9oqwFw7JqaGZqKl%2Fwsa1dHe7zHmCV58mwwXnnPOkJaRhTIjoquEMuhKZ%2FZiFNYj3h998RcjHo7MbopxW7MH%2BtIqz%2FWa8CHBAR9hwQ%2FIduL5XXPCKOJjcqWfL01WCXACHh546iOUMurYWM4PLMzg5gFPRVgsa5lt&X-Amz-Signature=8f4b61e28d6078fd6a67e0b29b86a1e4efed0b319e3001b1829d7f0e5fb69384&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:03:00'
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
