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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STO5A26N%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T030051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDreU6rZDcYwh6fO4EbjCALdv6opjr2rB7DRieNItuOHwIhALbYt5pU334QSxWzD%2BUN8D%2FTYHpx0YJ0lbLSQl%2FgKYM5KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwurSWkoDqcvV5PI98q3ANlaL1PlUvWttnGz6GynX4BmlIOyaG%2FnX0IxMkPsYvfW6w3ht56W7XX4O68FAutcWvB98K1G91hTCGtcCJ8sRxxAqOYtpnVfBDjH9hPwHXspmOA4tjD0fr9Fn8NEKYTy6NDB18aWfsePHQrj91zcM8y3J4Qeoyo45L4Phc0LQg%2FjUf%2FnI4ByE4sqOD9hft8DN9QchNXxQiA4Tty1TEFN4Paa32Q4zwaT43VoYTOJtzxZRNXE7aerd7uyfCJnKImbknGEy%2FQOFfWSsnRSGA06eBqlMs6R8HoRlC%2FA%2BJg%2Bd54VRvmMH0quSyuDlIpwsQuflSfeK%2FnpySSIcGWaMcRYswWLSKXq84W2cXky6rr%2BZDOhgt00PHSzS2b7%2FAWNRkQhwi1S9%2BGUA8p3VK4%2B%2FSNX7rfWYSKEwW8gyu5nQZT6E2fm2GR1x9e83CZiFesawVC1zEfpb7USY442Y%2BhLOUiClU8mCu%2B0V09ynirqC71u6Rzb49oMzzHl3GyNiQEmvTnfP1eEaCqGValttsS80VquEKlqCd72BQ0kqzZASBUne1OBSP2KW1OOytN1VRu%2B1qp4RMRcsOcfGEqt%2B6k%2BzzbJoxXvRaqIWOUZJILdK2LSjwb8cPvJgtAzzKyNPjhtTDN%2F6%2FIBjqkAdIH8eg3fgEZDY%2Fus0HhhdFYX7giRXPuMlouZqyzdkAeHKQh3pfBrm5WfxIFyPkqjcV1VnrH%2B8KCd26Om49QYGkBbgCvT9xW%2BNGVjwVIEo2G2dS%2BI9P%2F3hdxNXEjTudqvi6RucbONHEaLQAx37bksHnjyUeyn56bqzLy0HoXktijMihwR8z6oqaTlnivPQnaJ%2B1QRLIiZzVcNYhDSVREFIUKB5iL&X-Amz-Signature=992af497067426b66d89c5c711d1f0f5c07873a34e237f292453bf68297a06e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

