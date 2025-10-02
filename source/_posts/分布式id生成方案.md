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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QL34LJNO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDBr0mDCdxOt8yg8SWF0alHKcbngywBapK2lBtcNAowUgIgIMoNIkgmJ043%2F6LxxdAB90bMBEMAw4CACGqMfq7fiNgq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDECLajrhsLhsv5WAHircA6KAuGvVRYNPfcUgnQvL%2BHsSZLHNnk%2Brkm5tERWpa4slXGNnlJdZ2%2FKjMMD9bGVoxhPD50Utt60GLDwnt0WaxRVPZrDBlnRZbKGSF6FWWRaftZpYcpvk%2Fp320%2BN3eqmzvlPYQ7MU7yvPLP9Heo%2FmMD%2FJkLxxjP314QtAAL6fQD5jlnwfl9DWO1PUfDMe6GKDQh0KhqbgqyIZa8UXKFCO1EAyJG1mhKhhjAvbddLTiXKOYmaEqsAuU88dWKwq4C2AzBKIZpYbhRk%2BzU4734KDcjHpHM3peAZyWZuGw0dpQCofW25HF7q2oXQJZdS4Xnyt5LKE3ecjG8DARwE9MXFx4rZS3rtjX8A1kR5L2Izam7K2AT7hdWEEo%2FrXafl397JG3eG3YJCubLEYhB4c0NkFyj%2BVbztOch7GsPGpCOIapbKp5CYLpdNRkpORP5s4vTABD6uCg794MQUMOwDwcJw9rBD8KRpz6MEHMHCK8Q65DdJneoXlHWiTf5ufkhvIXgX6u6wiBoPouYQhNSv77%2FIG7H9X053T%2FntSnNhJ6QUx%2FNKWWOlDCtzdT7jIj9NaQKHwSrz0sLVCKrlaPLz3BngYG2ZM%2FAzTs2VG3qJzYWqUR93hCux1pwOW1BtXhBbhMN%2Bi%2BMYGOqUBSG0cZ4%2BE3%2BuPHVHpRg0iq1M%2B99IkHQ9hbt2bg%2BBWAkdT0Qa37VEkQ6RGejd3D3pn22vIjvm%2F64yZkNrfNAv9Z%2BVRfDEBFjhQxomhDmK76SuiCjG5W%2BdNSak5OQ5J6e9IeElZVLXeNYFT8zAOdvJzOD5lE%2BJDXy3kPuPKbvf%2BJ8q1p7TKKGgZ9j9GOyJNcmL7GpiILUF%2Bj12d%2Fld6h%2FBzmiSV0GSv&X-Amz-Signature=2c768117f6132ebc9709fb1e29f6194cac0ef91a7cd11fc5153e09349d592ae2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

