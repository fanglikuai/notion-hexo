---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666B7V7AGV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE3VWj2rZ5EuSch0tJg9IJ9QIFXORxlHjSM1xhmS%2BH2FAiEAlRfPivtJv7nml4IDSZP8TAFbVqKxVfSf9tWJEMw0sB4q%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDHa76j%2F8HDL28Q7VDCrcAxTbl43hv9kOJtn5%2BcCWku4n7lfAkEeAOmLjfu0JUCobRQYF9fH1yPPdXRYYv08QJNK55Kl0TrOSK9Wr4KRt7gDeADliv%2F9LxIv2C9XHEw36Qg2os3MewCGoxlj2ZTap5ppPOnEpK7GiAxqTzSJKRfXtoPqqbKfOS5pynp6BMiyMyY%2FEwupl1QRVNgg4QJiBsHnEgcD4i3cyohiMYHN3rkOpWUdMNyH3F9ywWV7hlp%2F12sdrUePtnlaW%2ByVqm6JadqyinD68g4A01YT1ANeLFdZZWSTJLE9SctU12xjXdyJmXRHRB3JBKaaP%2Bd5wylMt4iAi2u5q7Xp%2FCDmNmalN%2FQ3GM1FbOHpTV8CKF5lKGpt8UxGYdPgES0dmedIsazHG6NjfR5XSrtcsemyNxgg3YVvcSJju1myCFqBQY29aIa%2BEs87yXoW5VPX2%2B6eHPX%2FowYflCpe0AFxIUnJxmVLjp0bcq3F39M32jZkbUpGi6%2FmzcrBFULb5vEhXRmQAUMHoOmfEmXMJUkEJK5HEcWr9k4fmuGY3JZVwZ79JSLPLkQA9Kz5XtfZWJ2ZCDFIiYQKv8brOuOJ9t68W8wSPPWWIhwFokt14YWcYHqqjfGgoRcQzH2Nq0%2BWTjc6DJx5uMIy%2F6ccGOqUBaKpl%2FprCsVA86vI7PSWq6Movis%2FiFdZCuZ113AlcDjbauEE3nqpsr0EvkJfgUZeEdJy79Jhi8ksTkvYp%2B4Th7wvSgmmFFQCie18azJaJ1x%2FT8sSu%2FaUFRDI5WqD6AjfVLePYU00f7XMzvs8SFjndh15mZrHmrAr3L9K8YBsWbAcwqBibB%2BNAL4pBV9ZjAJaNHdaJAS89Mcrp09ZRRJqBmrhVc339&X-Amz-Signature=6976cea69f00bc3bef832cb53b95c76a10a69c081f6672d6cc7abc4746f1153c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

