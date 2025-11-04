---
categories: 整理输出
tags:
  - 分布式
  - id
sticky: ''
description: ''
permalink: ''
title: 分布式id生成方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUC4Q2G%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdAwdk0ur3TAc5S3uatFCVWbeXRijCCb8Ze5fOkzJXGAiBxC798hBCDuScChU%2BudC6IYhQvxbfEG5NrSwybvGTx1Cr%2FAwhzEAAaDDYzNzQyMzE4MzgwNSIMs1RdGeblQ6gll1xjKtwDCqFF8aSD1t7Yy3G4i8G4RQoBA4IiHPdYBHBSwn49Vl2bNoBMGr9mXHYAn0VnziF0BhtEuCXrZyT8A4J3f6if5T394iMkMDLAPN17FsZ4qBDirb%2Bqn%2BbN1NGszkf3CObx8PVXI9SRoyqc1Qdrye1oWpYwrgrAm7DjvntrvdC6Bnp%2Btjme8pYa87IqbbkW2p0aMDYZJ%2FzjNLeh4fRKXi4ogxeZoQ2bDh4p4w66fGdtkrrhtGb4W%2FF1GPQxwgOATCk3I4BwNtbFKo77fQ0iWGDr%2BfwZCOobRUCdIJPLrhjamv0NmiAeqsQ3JlcSLRLO1M1g3mvAvix%2FeBxNFMx4NeRhJI6ff%2Bb7LcG9Xetk7XuVMYQ74AF6menHOluB6dIy1tsaofZsuVbMszD2Q932vyXhj4GqmFLcEfO2TbriLUJcquNsk2NoiLTu0RVZPc7QnMx%2FfZW4qRno6LoGntezSVs5dcvXSd%2FXUNZr43Eh2eyjOvwiIpc8VOPBhCICp4vyM5SmAbFdmW0%2FKXH8Oo%2BC%2Fli%2BlQFNcfoj8lhWqkiDZTVKVH5pZ0v%2F4A9nk2DeUOpUAnekVFftzwuWYYpwdr8%2FOg9O1mL%2Fx6YzXP%2FDQdQZKO3mKB202CfWSyHiu4ahbAsw5pSnyAY6pgEnEcOydm8fYrF9xomqXCh3z7XRtLFyLqE%2FjbgZ%2FTbWXuHI4KgJb0itl5fcPFu6MnLo8AqkkMP9uT6r2zFOoCi3B6yX30vVEeyJmkc8alHJTc4imWPPG8n8ZaaNAI6k5djiPxCQb3EOJ91XejsWpK4N%2BxqUwGGMcXWLJRzO2vbIIhIrPSnZopv7%2Be%2FwlG5mQ930QKhH5lLAeY1sYTdZsvW0saVPi4Ua&X-Amz-Signature=f3d413a2ecd5fdebda914d107f0d000ad35c72d07d1cba68be918aa35c81e9e5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
banner_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
---

要求：

1. 全局唯一性
2. 趋势递增
3. 单调递增
4. 信息安全

# 雪花算法

> 0 - 0000000000 0000000000 0000000000 0000000000 0 - 0000000000 - 000000000000
>
> 符号位 时间戳 机器码 序列号
>
>

12位存储序列号，当同一毫秒有多个请求访问到了同一台机器后，此时序列号就派上了用场，为这些请求进行第三次创建，最多每毫秒每台机器产生2的12次方也就是4096个id，满足了大部分场景的需求。


## seata版本


将时间戳和序列号连在一起，然后每个微服务开始只获取一次时间，后面直接递增+1就行了


并发量大大提升。


问题：


如果超前消费很多，然后系统又重启了，那么是不是就重复了。


不会出现这个问题：因为这样的并发量得很大才行


![imagesb91ceb2795145be127635d50c861bbfa.png](/images/c686239b1567bd4f1ee7f6da809063a0.png)


最终收敛，防止页分裂


# leaf方案


## 数据库方案


去数据库修改字段，每次获取setp缓存在服务中，减少并发量


重要字段说明：biz_tag用来区分业务，max_id表示该biz_tag目前所被分配的ID号段的最大值，step表示每次分配的号段长度。原来获取ID每次都需要写数据库，现在只需要把step设置得足够大，比如1000。那么只有当1000个号被消耗完了之后才会去重新读写一次数据库。读写数据库的频率从1减小到了1/step，大致架构如下图所示：


![imagesa73fdbbf512d967222aafea71d8485df.png](/images/6ba8f71fc9902de8f6ead17de802a727.png)


### 优化


就是监控使用的id量，到达阈值的时候，发起线程去更新


采用双buffer的方式，Leaf服务内部有两个号段缓存区segment。当前号段已下发10%时，如果下一个号段未更新，则另启一个更新线程去更新下一个号段。当前号段全部下发完后，如果下个号段准备好了则切换到下个号段为当前segment接着下发，循环往复。

- 每个biz-tag都有消费速度监控，通常推荐segment长度设置为服务高峰期发号QPS的600倍（10分钟），这样即使DB宕机，Leaf仍能持续发号10-20分钟不受影响。
- 每次请求来临时都会判断下个号段的状态，从而更新此号段，所以偶尔的网络抖动不会影响下个号段的更新。

## 雪花方案


使用zookeeper，在第一次的时候进行注册，然后后续缓存起来机器id。


后面进行周期性检查：看时钟是否回拨

