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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJWBZ7QO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGlUh9F%2F%2F9UfsmwQQsX85df%2BscpOyX2fUsLY8J8rRIq1AiEAsz5DBgacCRL2oupMh2QJ4TtFUahlsEHwlM6LkWMBGuUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCgE%2BWnlPtYhpKEnrSrcAwC0J%2BVL9ciNn1%2B%2FDdOKZZ5s5l0EQMz%2FusbPdul77yJPMnsclN3gdr6Iirl6yJ9RPnSwVH45pd66aP3Z87eJ9oytXaiPsoThT44IqaYryCDjKU4REABQF6oro7EepNxpddbg4RhVKJ6lBs5dzmH4ps0M7MCL9pMeQPmWHnkccczVxzVMtaRI8durCWjp3jQ0%2FLPebqWmSHQTwqszGxQ6rlmic0Xxf4xHk46YNZ7LLfqRVpuPCprFbUFvRYLp6J4cbkrvwak%2FDqyu2ac9QoCIx3Jkx2Mu8%2Bv%2BHhBBf31LsWqd5HJD7qOp1GMzeavMXKCjoSZgS%2BYm75fY%2BEfO2Awh7q4lX3RfxVHhNAttSzRsiG%2BbVrl5N4S5NTz9Ar228keqZYKnKLZh5v0PK4JoL4jTFgdLQE%2Fk33HekakwgwEU91OR6xM29YQFvJ2yKZb0Dum%2Ff6ZIDOwj1P%2B%2Fafs7CyDrAinQWCiChZJUjTe09lK0CVDeEXVXo16w5oT2lGpFXTVb85BpIZefCGwpXAPcWYQxgyr9VO%2F%2BWXilWduJpU%2Bs4JawYzZNKWmAY6Jczb5OvOurLQyxmBnFrvugBLg20jNAIugzARJqSc453earVvCTGVp%2FbVx1KP0yLWzDfEJcMJzO5MgGOqUBZf2eq3w8cLBk6uQ%2F1vr7aAixydfekop%2FBuUClMkdBP%2FCUUFLwN27yU7MCLm17mxWDGoKjEeGNHJ4sVAl%2F%2F26hxeVyzDa2%2F%2FurcA4l75OqbxzlmlqmCh2gCrQZ8yNmX2l4WeqvPZNJHiNByE1LambW9beN66TMgoKq52DB8MQwQtEV1WiTUu7U9uZ%2BZ%2FGqZgYC79Oxz1Ju4geaQj%2BrXJyaW4VWeuM&X-Amz-Signature=fa8cbc5d23d1f14fcbed6c8d6aebb235f76a62cd9f59765c5ac01198089920a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

