---
categories: 整理输出
tags: []
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-14 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCXZQRVK%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T121813Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICsCt1v40cxtZYE%2BVUVHd4zXQw4IicogavoV2Re4I%2FUrAiByxkZ1445LNJABRUfKcXQN7MATM6LcfQQJqp2Ls%2BpaWyr%2FAwhYEAAaDDYzNzQyMzE4MzgwNSIMt4n%2B3TMbXVUUF3PgKtwDINViLchB2F%2FS%2BlcOB55ea7GhDWGj0RTUf08vKBEnkSEHlKf21TfwiZV84IVtZjBAgkA48W%2Ft%2F0hhSk4kIWh22%2FrkHF6xvXFMc5jJRjhYn9y8%2FqvLC4wJ0fKQpAc5zeYf8gtq6HH14g%2FRsarj%2FqAd2vVlqAJXnUumz6vLUopJZTv%2BhL4TbyPvPbkg3qRTIKXdmgIkD4runE8%2BbaXzasK2obaLCDjKcrouhmsYv11bUcipLMln8T70joV7k7yIHdD7arAqZY6UbJiIPQ0BdMk58G0iIaO9E9w%2FD70hYXSYA3kLKR0jeoT0QGr1Oh%2Bi9O4D0IIIQ8%2BR9PVN17CVkEGzlKyQ1C5uzqZJbC0jRWymSStRWVAvrf8tsxRd85%2B%2BiyEdKsLVSpHE7iitLf3nJ7pEtk2w4zvHN7V9KOkzYvo4u5NZEi0lVEix1GFNkdN5cckgvP7bagjwoPyZ6IY6zQg%2Fo18ne9S%2BCgeSVDAivBKE6YSZc9ZrbJ3tH%2FAGhwG8WsUDE7nPfN%2BZTaUD%2FEm8VwqKG48EqSLiUPx%2FDVFx%2FsNAdyEFjOcZZmqsiIBM%2FINTa76OLqU7TimKiv%2Fp7pr7dC%2BAVt4d3fiZCjF9rpwwx1iERLY68RcRUo5NU5EH75swss%2BZxgY6pgE38Pcl8bgkzpN8i3Y0y%2BeU4FBy%2BvuPEvNbAhFf7c96tIAuaDuYd2%2BEs87BT9jpDTFLaFvvPyhPR9ENCPZzdczBWwYUxco%2BQ82hDC43jw%2FruPR4lWmSbb136YvF60XmLV35mLlFdO5r1kdIQU%2Bf%2Fkii%2FDc08czTCShRGx9TAGe%2Foc1kMbHh%2FIbRO0TMSDBXoVYImigSgLF%2B1FYmNXVC1UHuQKUx7bpO&X-Amz-Signature=63b7e50d16baa1f74645419d056719d803e8b31f846c1aea02752dfa9f01d9e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
