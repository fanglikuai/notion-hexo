---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7ANN7B7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDMT0mkuCdKtxFOx1Spcnn3uRfmW6Kkp8cHGh%2Fwwkbb%2FgIgbtQeBw1RgyTlCtPMj4ZgHfCnvrOhmA1QDTTl%2B5iFYEgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGiFym9BKN84MX5zMSrcAwa%2B%2Bfwgh13I9bKNTGd5t5YYMW%2BbWN4Tp8ES4mEnx35DrWASPCK0bqREoAuPabHvEOfJwAKjhWnDBG%2BtreRrzjjK%2FGZrHkmo%2Bjs0wuwXDeQAqxgaiOSBnpLlVfHkxr9YSw2xniB2ywLQHRYmW8FF3X0Ujlv8dCe0zg7%2By2Lrw6Jwrief69SptaNvON0jeXPxBU9PaEKWqTEXNjSqP7WUvmZl2g0vKHCY%2Fw4XUI4KlyepdG2Azx%2FFVBQokt73KKVU%2FmNDIM4iOb9gZ8uTinPyqa2BhHMZPNdBEIKDy7kJlvMDQ%2BBqRoKVaDcA3riTRooqxNTeTBvoFhzJQZhSzU7rnRJRFH7mUNmpTFNaJp%2Fi%2BFQFm%2BTwPaNn7acD8QXEHnMAnuJZ6jHRvxL4un8IvyAu1eyIg%2BMkKstb7EhOEKr7zNMVVLxQWLg1XqqFtG4irbNlOTge5kLCpLxLxMQf3LtzVsnxrUjUBQyZQAp8KBiuNbMq1HvutDinKn%2BMpSfQfLPoIV9PXW5pCQgTfO3rbLLjEaTvFsdhzd8%2BAmSF41ZwsDnyQ5uGrRjt1oFtdVTLjoJIUqmruIgtN%2BZww88XXNmEPuc9osqgWDrLJ%2BAw6bSYUxqf3xApPDvIYLZ0RmxQMJbxj8gGOqUBquuyyqM3plWFpG7UYHC7nRW1xKfVc38u2dFL8%2F2rNc7WrHizvF0C8FjhPwymSJo7JaS8ElX10TokJKtghOX%2BKkR5%2FaRa7noWCfJZk9kz64Iamf4uYGtUkclz7da%2FdmC%2BRjEqLNgtOzod5Mw9WhsieAcPMhh62hrzd5zElcbP7rn9o5IUY%2BT8%2BxoiTRYQxJ1%2BgjVr6YkIF0S%2BxteLhYM%2FpEYwFSHG&X-Amz-Signature=36efad5fa553362ced87497c54bda4b3001d5f2cb63200aa9efb6d56fa522fce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

