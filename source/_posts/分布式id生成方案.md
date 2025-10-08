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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWBR7P7F%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T080059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQC4PJjKl8DIRLjVGzAbgwYu2Wfs%2F0Jel67huYj7i%2FOAvAIhAKgGSry9aERvGcpTx7YUQRGxl1KK%2B%2BF3C6To2VrpxRE4KogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZpnw7uqgAJU3miWcq3APAW69J8Ld5dU1sCs8JPjoWdBdKe1qw%2BFRGVoLrLUY6MZrUtYx6s2YovwZvTo4Cugew6HSQNDF%2BRnSdfSCFOGaLsWdLdxf8AXHB6sjeulxEdEqPIJaqlTtilI8IBlDJqjtGeYVhtc589ojk3zqnWXesON3SR9xgdDAcmo22M%2FO8gNbTDgVmHQ9vLu1JDRDOQC3NnOwadoUzeKnV6C9sClyMN2Jx7TJrHxLXiM2GhzwjfYwqr0Jt5zJxHipG5TSvojquvTB0GJT0lzugpCmlMWThhE7cWrP8YkICY3Q5%2BnFteUn8mC0PGTJLqbKLDg73GbmmPAFCZAkc4InEBIWJcaR7HQUhAEbx8KSh2oJwunAhsQu0%2BqCzwpk%2F9qf2dVeltZQnjMJmq7z0wjElpgHngRai5LLdPA7IqUvc6geO3JejgISyEl2pYJ4mEw6Y3%2BX6KWVc%2FdlRnGILWuXpz1w2Vs5QutalbbGGs9N8bsmoxj%2F3MbYuGfHLnVonHfWEkMEFlZaxN7g701AMLqMVRu0Aq3GuBw8%2FhOvn4ADv1JCAuUvIGEeqeFLXvYdVoOlOBAYIxPeMansHoc8GwVYOCNiBZRorTRNv8ukFxxAhad8nXzBxaiqoyKzc8rkwUEWFWDCsjZjHBjqkAV%2BFHWuN9xM43yX%2FyFScn9ddAMh8IltVi%2Fti1AtYi%2Bq7I1Atqu5lYrNZogJ5OjiW9%2FM%2BLf923neVKpWtnmyAY0rqT1AeNvfXxNZokQPl9dJACLDzxw01utT4rNcKPnkfmM5ODsj2zRUVPRBSgOvLAFowWdanX8dJhq4TM1G%2BGGQ8sFkRsTzDY3PBBtCn6CiawFIDDcvlnwn7yx69ZYuTcdNv7bvU&X-Amz-Signature=cc3e2364712e558b5766ebfc65e8ea3c1b3d437c0b4a79f612af4f39055c6b91&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

