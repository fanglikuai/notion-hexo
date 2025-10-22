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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGM43ZL5%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T110057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIGjElHzX%2BGEzbMtusYCehh1GWneE59H3PlN3kzxFuZHqAiEAm0iy8oZqMZfITR16Ue9TENAmhj31ZAD6J55bxSpPVsUq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDPNo2X94h%2FUPfton7yrcA91%2BkMYVj1mJ53eckqusNt2gWfRyo0w7A6bxvZOq6hsHri%2FCPJWyQ%2Fnbmbwn8MIxglL3rtv5TiRj1P3ZQk7gbCHyxr10G8nplh5%2FEMEwEdDZl97EWyPemICnv2tQZZlqC446up%2FZxVo8QUe33GgvQQghoisnTxCiLxqHVLqISH74jYWAupwcF8SjP4ZjJvpdVH%2Baz1AuzpIE1tBhixbKZ4rsf%2Fxc6cR5XNQXivhGNFRM7w%2B8peITL225Ku22PlcChypppO8rJJfy05Bsv%2BSOgH%2Fc9RO5Pel5x95fc345Hbhk9t%2BLR%2BVQ1I4aDKBs3zK6LvhBj7KPW5tieKLni3M7JKY93%2Fr5g%2BixL2GhR%2FzTOTUEtvAFeIpLLVHz%2FrCfNYoVg%2BYlDKvPuGmb3bwDr5wn8F6aF2260dechEOnnYzY8IRAKkHFzyuXtpY%2Ba3sljsTdy2KIehtRvZa%2BmkekFH4%2FTjIGLUbpOERDmjOntOMHfbXkNZ1J0PMZtLI62Z0M2sa6hpD%2B2RgdrLSC1J7uNsebnwsEInwQY67CQ0OlP7913vOmnThQE7FaMP3JqnLS3zhsGDqgTfDN492yyHs8OLYBWLsSx4ITlJ6XftXGGb7N0jTenegU69hPwyNFd14vMNDb4scGOqUBq6rehitC%2B55jShRz2hsFJmP0dyJUP9ddvQM%2F6KcGxSp%2FNeV%2FPulh6iu%2FNPMafl%2BZ9eTHq1b6bnEpF1nnPAvBPqBDOvxjJ01x9pqT7829lpdMkdrm1uCmiTqqWuQ1KgTeV7jNhNYEXYZAE2K9G8peW%2B%2BQXTuD96azRWjdbdhfEwRwtv8%2FJsOy035iWicIGHeS%2BBlBh0gACzDlAKBGWyUBNUQqNn0g&X-Amz-Signature=8c8d09312d3fe2f9a9ac6ad8a5a951aeba06b244445ff5f4da702af5ef79e5a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

