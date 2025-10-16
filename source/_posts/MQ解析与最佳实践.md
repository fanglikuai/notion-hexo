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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FFICMOW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6bSwJMr93rBSr32EMDyDQ%2F%2BMbVeiGlUQgAvLIF1I8zgIgPyWZf1rzBgP5ypyPzeGRwtWSdh2ooPoYpe0oPZ9bx7cqiAQIhP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJh84FUlx5TyDbojUCrcA%2Fk8X4pMsXs7H9HJ5Wfdu7WIze6w4esOzHX8JIWUte9WZ%2F9Pv1FZF3CGNXo6KecyOwhhzdRTEp3w6K1xc8a56uYWJz8LBmgAkMBzTmE86uGDAdH7K6smku6CvDQxFVtlljJDzfwaUyFKVs9UFE4j%2F6N7NsE0ZaXssB0XMmhxDQEST2zzqtUgABJVH73ByerG92CzSe7WxYZOLltFcGLS7W9y7I4n2GUHVH4Sair59vAwo4BrzT0ahXtgjTCHu2Ur9fjZicv5ouvDRt4Sa517Un57DgO%2BEvmG%2BKW%2FJF8ZLzSXSUEyiatRczUoMLRtPx2je%2FKgvq2rXXvFyfPyPhOWo6kfZ1Dm8laSyt4s7NnaIt%2F1d8wkKHYJVn2KnjlAdkNW8kxrnsQSJ7NXsbVSBIN833J2k%2B%2BE0pOE6a0m0kU1amTwzkTAkRwYEd1nW54tgr7IRtd02xtXTuuhqjHVjGFrrrOfhkgADyfMtcPA63Le9WZI7Vx9%2BBEp4EWZd1sr3Y77zJrLUdIgYZujfPg%2Fhb1VyDB87x7W86PRAaDgeEPpEp%2BR7ExPP5rRd1Nom9TiMIkmXpUhMi2Fr3Vlmm74dCn58gPE9l952660DBLTZzXhe5UIOg%2FOCLfxTM7mKrG1MMHAwccGOqUBZ4C8XHLMAkKTC7dFoZiCgGTc9MfDzvb%2BTLsNIGlCotLZbBv%2BCAd01Dyr%2BKQEYtzNVdkhtbMcl9URsUFBUBGMrKg01MdAY09V8sFQf4O8%2F8q9NGhJhbnjOS%2FzoCOknoMF2M926Dejk4naDtqWWwYoBKoRE2VbfnXHH1jga%2BNy16sCXMYUSwi%2FZ3iwc7KiW97Q%2BVF%2F8gLfEkCV%2Fl%2FAz9MofYgLzqbX&X-Amz-Signature=1473045dca69daeb7805ee0e166f942c8eae83c0431a0c0c93fe384e4ee56028&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

