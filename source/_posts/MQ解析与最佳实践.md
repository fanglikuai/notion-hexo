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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFEPRFWM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCOLUeVoL3rEdAsrWq81BIffmnQNFRQfWRamrME7kMlTAIhAN2mrzdtgw0g9XmK32W1aLFkt%2F3q3Fnm7A6msracTKsOKv8DCC4QABoMNjM3NDIzMTgzODA1IgzZQ4KvGmp1irPWTJEq3AONfjAtxUu%2BN5J%2FVKJsJ%2Bn2jeX6WGBKBedqR2AXa%2FbpQXH%2FIlLTGSKOewWra%2BZ%2F0RnuGohxg9Uek%2FBbivHIFwTkfRuV9aKLYt%2FHN%2Br%2FHkSXocEcamdA%2FVSNg6RA61rs5UYOA%2FvfdZ5Bb3yUVDeuDwQYapGh6kAXgr3rSXczRJc874XlFDcnT7Zw4Sp09Vt1EHCrGdR3jUljoOVy%2FH%2BYPSJ%2FZLWgbAF%2FDXpMkH7m5tdkh2vIRX41O47wAfYmcPGkPb2SJcifPbu%2FxBOGZjH4sPwVxR6U8H6%2F6oSuJ9UESSNZLJQ2f2v23A1BZ3jU%2BztfDwfBW1HLFaZTjrpnOyq1rBvFMvyi9sQCXCWcomoTtvKDPoEE3BmY8JAWgbO%2FYHUAJ6Y6dHEC69pthzWuaEPG7WD263ppuwcN8raEpYI9tTvLdME9WRqIKq5wA%2BkbPwCLhrESqc4bYLl%2FdM2QuuXC7dLwO4CLuRPRJR%2F2Gy1y321GY%2B88%2FUhjpAbvIGnY5DMRNcrVw12xOv4pX9EW9GvjE%2FEOL%2FZIGO5YDS3v5gUGQx7f1Pjv%2BAzf4XjHm%2FbqQ%2B%2FdZ%2Bs%2BQ6paJFo36YgO8meb3b2re7lR7IRXVTEQxGsAETDeIENSwLCznGYceB2NczCSp%2BPHBjqkAX7wMUaJMJw%2BKjdTxS8%2BT%2FD4W1UOLFjLDrNWdz82S10G%2BuCTVEkrbBqmr14A%2BmgcIs0iJeNn3%2Brdc%2FQwCpkRvIXBHywpnUs5TEiwJ70nWPChLkm5fDzlVrdvivuyZ2Bbhcafgw6TZHKIXMw0XjBfBTqpeNAnYA6gv%2FkeUrRjeH%2BZpDakHo1diHhVOteAuBl0FIcGBetuYbDDa%2FCEyg76as33ZhBT&X-Amz-Signature=302dd23d2b6ec7622685d01ae059c406eaaaabd5c3f0a2980c759d6c463f8bbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

