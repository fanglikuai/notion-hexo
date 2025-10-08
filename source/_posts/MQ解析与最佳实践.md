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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQC4VN4N%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCpnAAVT6ygu0rPqn%2FCS5b0IvztTd%2BGuEoI63b%2Bz5Xc9gIhALLnhUVhOhbZOkQQscOlKEoAi1mhqowuhCbuxARF%2F1ugKogECLT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgySc7aG8ujihauKkbgq3AOyk3u9XRvNFx36uwHrX00szPlSKez%2F3Qd6WM066PdXnNFehb7sgUvT%2B1%2FR4VMfutR%2BxYjaW4z7ndKacPk58OnBQ4LOEQNc622BW4eESFqcObp3dNnWkGnnaQzI2EUYptMOJljY1JHALyKmW0QsxPz2BVYFPS4tA2V4quaQ8At1e4DBDDRt8jkiZR1uLU4JZAE7Ezq4sMR%2BbkmliMFofV89AylCefaOnI7uW5al7NqPXv7FTs2cFgikHxWaZa39MaFLJfAzt4cABg4WT6lpbyC%2BySs9IK%2BU1CEXIWuuZ%2B8GhyFv0m8afhe5bkvW5FC3NtElaTZsmwqiV9QSk1ylTLpNrvqAimYMyWW8C3q5kOSVsWYz7vBEoiw7TFpWfvTapA1SKctgZk1XQU4mXRXauf2W4G22iWVptgJaN%2B9j%2BrHUh6orG8J8XVIxJp3iygTiUaLgstopCRickWRN1ASCq4CviZKp9Rcyed7fjlVmDj8%2B6X1aCcX6dlrItgYiQCibXMLhzsmAeHF43gENMPOA5LHlkLb0GGBhsThiIGrkwa96NEotss0Xv3lH5TR3MgaURQTNKJ2TGepsf56twDXaf4Ied37%2FW8a7eu9JQymAx6jmWUeeS6MgwYo4hVaCYzCcr5fHBjqkAYtkRXfREsA32%2B5S8%2F4WxVSwIMYmcftHujlfbltk5Owa80TC4CaIcY5Qpn6i96BuHMrQ%2FaD3R3dISyng5mrs1n28QPb5ywPZWfTHO7yOztiSJHiUFnIo1Awyb%2BY%2BstB39LfSgtKQ1hyvo03%2BIRio25TI4mksZPJfL9krk8PXaweGrPlUH5Dji9tkG5LvxL7kKuRSyHWrA0%2FWKWRXxlPGzHHuTYfX&X-Amz-Signature=e161dfd17e3e23623acbeccbd9b533b390249bb139b7773ba7630a7baa5670d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

