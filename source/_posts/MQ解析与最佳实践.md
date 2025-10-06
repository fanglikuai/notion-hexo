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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664IVW2PLT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD3qX62uVohAFoK5hj3kdjDnOxiIzlfAR8FceGhLvpFFQIhAJUunVFnMgUCNTF%2BNf1Ad88TDjXvKXnhlL1LNP7qRUJ2KogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwMlFFYuytUgNWHV54q3AOvZh2ljOQssSbrjl3LWCYiaN4JkBKKIGP%2FKWgAGXLQQgJbZpMTNIfzu2LopMNXTnVhafrSKsy1%2F%2FVXJyTPCl3REudMNL1Dvw%2BHPsPMMIzXZhwK19n975FP%2F7zh5I5dGWz7t5e25%2BiC1zuGvI3ZhDdGeYioW2zsSSVBAuxxKY0ZHd58%2FzTDxNIF8dh8weG9mouwMcG24XfVczaLbULWEiOkdv%2Bmu2J%2BYgCBJ3MvUnxhib0ev0%2BtVREYKrrb0atWmGziLb29YI7mEISJJe0KxxHv84WTMgbprWqVw8MFqOkrN7ReUBDT0OyIqKP67P%2FBLj3dvrBDMSIsqoYMdTx%2BFVRegAyBEyNN6j0gzshLHEaG%2B4oB%2Bpzk1QX7VxB4yE7pe%2Fk3dfKDZ1pDQd9OrA06TDeYH0UfymjKgjL%2BlDxwZpB8hZCwCYUTKtUoWkRE%2Btcz007Zs7k5A7AKg2dLnmGENWwgfBvXfJ0BuLuwYse3x5y4HNIr7fxBl7LxJtJrTfqUndtiFzv%2FpSY%2FFLcQcX7xVjiZuAefcPje3M7HWv4tqEyacqdgjQOttudtB0ovPLC7ZWXFnf6ale9Zmfa0Mcxy61P7KgBBmVdK7QXhx3KWBjVOyhQQIFH9GnyOI8w7YTDWvo3HBjqkAXeNc2d06V1WtC%2FhO2CTf8X9%2BsM85N%2B%2Bwj%2BtiC8BICT5%2FDc6DCGIe8U%2Fx6qxmkGkUkcmhFEnpbGHlJBGtud9QfbxPmHlTuz1IgPf16EGe2OKjTTo%2F80TDFO%2B%2Fb98qlzpaL%2BJJwPNm%2Fh2Py58GjNRhcg9aS3xwvMsJgpH0qHle2zUQHWC4ye26tRGLfZLHHtRz00LC2udGJMjJIEN3qbfROQXF4mI&X-Amz-Signature=d0aa304aa29515380e806f0a5658e8e49d976944d435b80bdbb63b107da2edb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

