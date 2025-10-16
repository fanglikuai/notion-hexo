---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EGEWTGW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrMp5pIUOG7NhUJVuqSSw%2BtfxzmkVp%2By1sTorJesFzmAIgG3shvmMhFGNp81r%2Fy3DYtQBkwhCECwG1FI3F7acQAxMqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCae8VJn681akvf6dyrcAwweHxfCS9Ngs9ECgzxGmqFVbklvQSg2LB4KsB7cHlcWebARVh4poe7hUuxqVqbiPhAm2Nz%2FGKfUArCMa5ksntxLGtd8NbOJ0bw%2BWQ3pxaqjj2KSWPoK6gdhFjhZJuGUgLKO3yaFDPbflDuGHQ8NoYrMzgT8TzFW04C6w%2By5ktNRRcJg2s9azVtZjID%2FmD%2F6%2FzeRqtcf1675XQo%2FqCCx20yF9NaFKRVXULjxdvRRzqJGZ7gzIT98ygk%2F8ymtV0IV0yFfvMoiUp%2BOp4endbnl81Fsy8pnlCAN77%2Bjh7u5NDjtHiOyNDcMqvdIgjUfDGMG83sx2Bxw7N8NSDM174LOEgSYj4Xe22z4Rr%2B7qWCPs2V%2Ff9NX840uCV8%2Ffmn2ieIYfWN3LKLPO%2FUM9tWEwieTXlCH2zpU0OvGss9fhdOAmaTUzhyeOgoyKGoxXQcJCQqkC6j9%2Be87yabc%2BI8LOVjA2uTpUX%2BS%2B1KSLxqWIWuq4ymwM%2Fg%2FJEuC1vWzdd%2BzvW8qi1NKzPQwzZuW7MKx7p2DLU0gtY3VKHHSR7VFkeCsW0EjtCbrdgoZtpcFErpKLJdHCtvWzVsW6B8ZGtxuCsGhdUdwmJ6jpgtkjw743LtIt1drD%2FScGwrqXD3r0pmQMIDZwscGOqUBjjVzNzhFzW5LNeeITFyoPXQXh4zfGYn%2B%2FeIR%2Bm9QspIQSQBXLsoBxdToTYV2MhEB9O8XpBP7Sgue6PE1RTvPhS6XmgLJb5K8KfW1ectQ7EYqNo0TyWidVT8pT76gSBOivlxjYZsFS8nT7mTXQQ3zqo%2Fji%2FsNeH21EgkCfXcXZKMY%2F2xxCk2l8WnzIHDls8r0qsXl7x0aB3fUlwPp9HLF%2FGi%2BdwhU&X-Amz-Signature=f83f1a1aab34e6768e9af33942563b7e1acc308907c9e9d29a60b451c4a8a687&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {

    public static void main(String[] args) throws IOException {
        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");

        // 2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();

        // 4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);

        // 5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

        // 6.释放资源
        session.close();
        in.close();
    }
}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

