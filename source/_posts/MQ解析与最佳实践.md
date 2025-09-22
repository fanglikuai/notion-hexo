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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKIGQ2GB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEKR%2BZbxdrFhOhFuhC3EQ5B%2FWUWnq1b0TKxWGV%2FeZc2WAiBHkuB0goF0tIFtESyhFzF1YLkcV%2BN3CO9t9zYGB8ZO7Cr%2FAwguEAAaDDYzNzQyMzE4MzgwNSIMKpYhLcS8Cn6aFNA8KtwDrD9Kv1rvgTH%2B2bShWGiCqt%2Ff4IRFlvTQMAH4CNBqEONhlmwKVin2cfVU7PNtjSAFLIbT0nxvQrSNWayfqT91TrgiEiwiDNhmo5e2%2BPFcApBXX7AJ1KTtbaHkiuiWdHNBJIWGzb%2B9Gk2ih1pNrnTZE%2BfIxBrE8CVHnuY2HSh54BtnipLcGcKdQk9GqY91oB%2B%2BDMDp%2FwtuUkCdvy%2Fkh1VJjtIfOZXv3%2F5e75nIo3PYKM6Amcv7bEK9S323spOatZZ%2BxVBsqaxUx20oROB6XUzoOmKdMyujDiE1a7lOr6d8WxsyJW5iNdadFonzvC%2FWPIDFL12PZOlbvK1kySTMHpX7OJjePRHLMNCfVRQ58tASEbfdw0E0%2FdJ4o0%2FT%2F4hOR6qpgv8K9DuWZ5kH013wXTJAuydyuh24T5oA%2Fr%2BKqy09zW%2BDnlZ1jsxNqVK7U5WoECaf8MAp8A2EAe2zezgLlYpNPDOFzVt0nvc0Qf1E5uhEGYyxOlIRMCUFcqoV4gbO01N7ecCHgtIyK0AfngYbKr590ONWWuAJaNG0qIAqyIqaaaBxULfaNEfmptuRF1MgVATX6e9rOvV15QsfX2Ka5gsb%2FlYxGjVyDvtI6lJEPilzEjSWGNqaEPd1%2BvDpmegwj43FxgY6pgE35fUxhqBX%2BzITySTORJR8IGd7SLKdAeHqnP68m4eZRq3ARqXA1BC698Qr9lqKYliF2paQRteuXVSJ231zgygn7JvUwmHxuWp25eRgKtQDy4aM8SHJUotphnCngVkwrasY6zXjGN8zHlhdFCs3SWnULdxmtU1F4hAgiPV7Yu%2BzJz4f45jlcd8%2B0PyQy1e6nxG7VpJpemKFnb69l9NhN18WleFFjprs&X-Amz-Signature=c5d58c09df8fbc8f66a63880bb55d52391b39e957a225a8aa1a7dbe40742b525&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

