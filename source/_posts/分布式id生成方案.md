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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664U5THXP7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T220046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJIMEYCIQCwjLCSFnqqouDLskfWez%2B17SrFfj%2FYjjFyDzycVgaE4QIhAKtVPw47FxB5XXVLlQo1OqhgBJ0wul11M906jqUhGeE7KogECJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxvitD0760er2noqlAq3APlxiaBF567IWiZEvPg3XgaFPLgrX6lGN6XADl%2B3174KPKbmM%2F9uNmGv0diEi4F%2F%2FLKGUeET%2BAy28%2FPG6%2FQEW0DOpCuBaKlTFcAqC32NlNp4ZzKik5EItDps%2FR1n2w7ROCo3QxsSAMGLs%2BzDm0F%2B6rcnQg4ql6YgWAMGpgMzBGBhnbyl%2BMBH5r4JKkk8D3%2FcZ8a5%2FEvJ1CMJ%2BaER82F79kJnHCnFwt8yXa%2FB%2FoLULhdNBwv7MVyeG3CWdGFevPtjxhfRnpZSAJKEaNJYIkUFNCZyqIjsbTzkY7VbX18qeWuI%2BbSi9gpbW%2FxtFrM60P8ewkzwHtxaYW3zu%2Fg%2ByVtdpA%2FbQ5eR2CTfaKx833%2F175Vnf%2B4fb1Ps%2FFUHTdvciHq27O4y5NYYI58mxWDpJDPjsTiH7S6LkhGT%2F2rmyDnOqW5JjGUAjSzq%2FzkPM%2FNn%2FDEbQvF3S4eOs5ol2wEBdUu4s7QclNgAg3RnCtZTGJFai12SS3Q7f21yntwcGgv94slp5W%2BT4WtRqbl%2BEs63nj7Zz98eAjIGxWSj2evfs72x9DFVMcuf057jZmkxa4LnZV5dPPZE%2BQ4xpjEph5VlL7gi%2Faj9YItNwz7SCGe%2BBPKdYv1FhHIP89xEOdKf7eQjzCooKfGBjqkAZ2AnL3VxmomO3rYGxqUrBoooYDIYmzEJWE5mZ%2F8Zlw7gIutI8SBfnOPLtbYWTq78ln%2BKJLR9Gm9zi8Ir3iw3FBZB%2BCI6nUOgecUsnenHXHiQ%2BCYLXTsY996a%2FHWPXV1hxbQKML7wU67O6BnL92ILN2Kxq3yHvhOm4Gd37UfvgQkWELFuT8sjNWE2KoRhq0KyEiNqUfeMIm%2BIxdAFS9yGJ%2BtyiYW&X-Amz-Signature=82734fcab5bb198d86d354b11e705d5aba3ec2bb695c061d4d5a6cc7931a3083&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

