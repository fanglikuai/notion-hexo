---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662NBAIAH3%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICM6DwVY7S2ct6FeknhdnA%2BcdLJgIeQtei2TFdnf7ilsAiABi5FFiqBg4BZGG4aW36pUPVHpmghhMr7RhgdWn4gIXyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5WCj46lnq8eMwhz8KtwDEz8jQ%2BpWiNw2ccxS1LWsUtiGUgUeBwG7C4V21Nf4ySoBF3s%2BkI0D7MVx3F%2BApLxHuB68ZPWdrBYK23vir%2B0BMzIhIRGacFHL8t1TNKUnOlnNFJcQLKkAsIXWY1iRpixM6PIHfKTbboaAgyDnivtaeB8oSsa%2Bp14qYj7cVnyPr8Nowc1gOtrKNDn4r%2BohRhAryX5Hgx7UoFWtbI5dVMgnVyzDWFInWc5plfHcaQ7dHVlwzhjcEXZa0zMaOuyWD4BpLzjQPpn309quAyR42BPZugxVcyea%2BRvXuMZrDjtIKrIaEN4YoKOIa%2FGwlQowpMUrpDUjJlceTaT8uWff7OKbUSpamgm3fuLVKN8Uq0kDL4L52iN2h4FgWKr%2BVSGBiYu9f0H7hGBsesbEz%2Fdm6x%2F1GLczFgdpxnYVRxePnWQoYQ28pcQZgVXej0rx8Cm8gf8kA4J8AUm7cs%2BAi2%2B19BUc36OsSQEOqEF5sw6FF753HSaulHRULzjeK8W3nC3cha7YxJBIzNQv4JcPwTZlIxBizgfLLLTWMiDW%2BtkbAE0A8Rn7a3wbz3MMTOWPbSxCsNH%2FTQ%2Fqt9%2FrWu4k%2FHJ78IqFz8jSBBdznCOl3knQNgrYnZhurGd2jj9C6Kp4EHAwnrukyQY6pgFARS6jMyGd%2BRK%2BycrYtO6fQYuQjbI1vEoDQT%2BAUtg74W0hO8RFwrbo%2FnTiAAP0VX3nM28fEcYwxGBw51SDiIf02OTKM10yX75EJgeprNpI2NtOAP3M2hb0iGuEg1rb%2FlNo%2BP77mjYZ7lEhh5x0sEhSvZMxZg5oTUq60bLqZhXkSNvtQtUYI1m0B61XW2OeKz%2FMvkdQ6da7wyZRkSC1%2FycVS9o2vrgg&X-Amz-Signature=3b0ecb67b02dcd645a54cdabda378dd86568cf2936cf546f772f3f1e0e9010f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

