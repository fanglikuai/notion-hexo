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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VYSHATHO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGsrH%2Fr2ENmOZmlCA6GcGYjVzWmzwT61eYVt74N9lQz4AiEAyAuE1IkqAHXqNu%2BAmr2JLApotXR%2FmnmTyqBKqPo9nfIqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAj98edp2kPpq1kBcSrcA0bV%2FwKX7Vki8qB2OkGs9Cj1PQGyaDhTTesMPOsy9egpSNGAMSOZGnCfMWG7d5YkLC%2FCCKlZ%2FWs1JZJNdBhfqRvS83CC%2Bw0%2BsLtJbMgrmLX33dMblXvaeBfAia47OsSknW2Ggk876zkG9nmZU7HOMy2Hy2BR%2B%2FNYD8PhuAj8qcxUklel3DYmtRKc%2BVlrGp%2BMkGxyN4LP2t6id9zPK4TZKa7xBMRlYrOkrL2IlhXky9evopi7ko6BItw1XGpSnqqJk%2F8ItazVyFwn2RHtd7GAEF22hBTSXxOOVH76wAc9Sjhop2lKYXwkKTyWBALOHvxNoSPJ49NakYm1ZnRgr4cDVLyok7t3Dk0uzcsP3PisrNx5swF%2BhDGocwXrE6MaShjZqGwPDCRI02r77WfZ5r51EmegxXm7eLOjlhbCw81BxrMtYRRE7r0Q%2BBLmLzTX2V0goChB5RE4g4P1nj1osOezVd%2FKlxbUxvJcvx2tQEfXK3zOWl%2F%2FU8h3kb8EjBXRtZ8syKhShRi9chccyCO9O23Im4YJjgHVrt%2Fnc%2BUfCZ6Nzr4BvM1FCmyOyAjMxMApuiS73UlyXM5CzwBLC5rRQyTA3ttVp3IrwgtAZGMEoza91c5NYajsMLVkgKh32D8PMIn%2B5cgGOqUBWNkA72ODGPgzgRLsd8Kn7bBTZmDwcfrBwx50neeyql4KltiErZyCW0021ZrHCnRrcpivZB7r%2Fc3lo88vTquDavkzxGjVrq1FM56IHUNVlK%2BINO41D%2Fq1VB4gnmi4yBn3rqUYXP6Noq6MvlVwcjvgpR3GM5PRmUe7mOVWbzB95J0Vf3I22%2ByP%2FTL%2FNq3cvkwJjsqraC3VUf5Y4HAkQwMDVOZXWhqD&X-Amz-Signature=195035001004e491b3fc7fa69d88b7d66b431d6947c07e612510618bfc5a3049&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

