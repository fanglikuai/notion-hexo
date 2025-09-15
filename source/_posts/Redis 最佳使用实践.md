---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSV6MYGC%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T085604Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDHEwDi%2BJbHCY8FhpsvcwCfrKkQWVGoDWOJOh9nPRse6AiEAsrNQkx4YBKlz1LDmlCJGtFr27WjpqB%2FfgOJ6vOd66Nwq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDNlPp9SpLmXHHjoVSyrcAxdqTdVruFbsjRIED8CdHw%2FqTEDxJo7sutGDnEY2Yi3wwvqYelQlGcrOSHg1%2FNCzHgYOl%2FFF8TyazkMjoVaCm4xDXJIqgeicM3hxxw6LQh%2BQH%2F9Qx40Q7%2BVw%2Bs4qdOmrF%2BrKM3%2BI4HW09ROlW8kkve6oy4g4ud%2BDnG8tqxbeReBeICGo984n7hFKH4A3tJJZUFgF3kefh4UgZRMhK1ts%2BAfsHDQ5q6c%2FOPuSjhQf7ySNdb3J9jEL7BCJvGYmkwMvX0vedHZsUVqznk4snfMvhjOU24pAgo0PXZhLGNANgAFsD9VedFzjrU2e0thZFIYoZx4NSohoD%2BrQXVc17xmycd0t16yOCwjb9UQObp6pm15nd48cZq8sgJDsqfYMFXDam6K1bPNQBfvDo%2BWPaTMnsXN9Z4Cu4JoInYMQxM82eAo6SOo5G%2BAE4vu%2BmLFIasqUnf4pJDmvjaiPVvTIWbQfrbr1WQYB69YWtRgdWbmPwf79OClZW8AmP9050a7tpoM%2BF%2FwwM984Y130Sdi4etMuIm69g0QOFiPQvhXihPNnMST0gp0G%2Fl%2FMyGAKwxod8%2B5Yp0NCkzCkkvVzXgyk1%2FnEQ7jsYEtkpWohczyPXwZqn%2BzICmFxW4JsWpCh0rcvMLWTn8YGOqUBpdavTBxjNR%2FohKL6fgbsGGlHilllFXyA35BgP9ce7Jrip6SE69hEb5D1EfVkiJ0Lih8pATFR1X8Rd02XHN%2FKEs78usgh4nQQJIsLVQVECnmJTLqJDewtPAQ8193UFbQt06YIlxos0hPNGY01NkMyyByoQY3QiFGWoPjLKLA3oXGhncDQy6s962vKXnatXkiL7VN0nCPvDznkUF1xvETPAfZ43AV6&X-Amz-Signature=91caf41272fbf6952c44a739fb86e50ffc056df127b52c4c875e1bbb7c51a68a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
