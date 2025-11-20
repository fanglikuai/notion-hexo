---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6WJKTXK%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIAgVixZ1o%2Fy1mTbpqN%2BZz9xYNOzw6l%2FiAu7Zu9%2BZUxjYAiAFclwlnu7%2BZc%2BgVKRdtqJ90evDN2kt3B2qYCDiG9cMVSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqYvZ1sKOMpkSnCfiKtwDA9%2B4BCA7OimPE1BUnnl8rug9bWMF7i0M%2BEf7HtUXYAIi1U0Wpb%2BjAWndj%2Bn%2FHilJQIV5We9AkdCVfzzpFmBxTiOgVAxrWeFyNKWmnU0gAoLqeToINq2F9%2Fc%2BBbaBr0Ho345NlZP%2BEJxDc%2BaFgAKMRZO9R%2BskCPc3LT18uIac3znPoEMJps94eNXI9Z2aKVECnrOH8eAz%2BlfYSgiBSkPeZ79AOi60q9OnR2XgTZUJVclXjVNhQlPbQDzRrZ37B11hKgqd2YTe5Fn2i1dgWoZ%2Bzu9KTbjVjQ3kHJ7Dk5e7W9wvVBUi3a%2BoPBxyZCT%2BbeaAE4a3EeQWrW3hd%2FTO%2BFlKFNclXN92NqKL%2Bsy8ZWFuCiTBQfnOrOpT6d4sHjIdkAhEa0rGtiRVh1kFAvhTqUQDDyHMQ%2FOK7HRHzoqWXsGyijAIz66mPlJiuMohikrJGRqbXl4rndupBQMStKUkCaoRNKbS%2BzFDMbp4d4WHeLmfR6kQHCc9MEMHWxygwPWfqhl%2BiHaLVNSe1HkYiMMog%2FtWfBriy9FbIi0hwFMOomjcEovrwM%2FOFkm%2FwXniA6tDFLj9iT3NAIqTm%2FA9JcBWhvGGwIARyy9EoykaYvoZF49cC4E4vWbkYmZp1zLiUDAw99n6yAY6pgGyaRIFisf0MLUprWgNpW1P4oU4bLK2eKu5JDtqrHxwMf%2FdhJ7uWueybftDYZJbFlCQLdUvF5MG91ZUvsmPNwYUku8kSDLwXjF3jOhaWOZ2gA57UfcBUg%2BcvQtDQKEru2JyfXoExQjrHdKbOQRRLV9NB5q9FGjlp9fm3h3Guh8DmEFHRjjhJPl64yK3qMZClkcdEUTwZS0Q6RtslrVf3aNQE3tZ9EUh&X-Amz-Signature=ef588cb4a6eaa2aecabc11e7ede1eafbd27c8ccbffd4287d7529d59ccd6a29ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

