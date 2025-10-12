---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y7S2YMKA%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T060056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIDuhl3Y7U5jMtXU%2F4zyS3ycHDkZWbWE3bbAzALMussCrAiEAkU7%2F5OfHloEoQhIB0x0ChzjiSUBa65FhZf1iPO44a0Iq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDMvqm9NLdxZS%2F5BxPSrcA%2BNRB4YE94Sn2%2BVEVSYOl4g0fO2rsZqKYQlIzjGvaPwUfxfHzWqYYnlxHxcqngiB2af6OErAsufJAA05Di5htY%2F159p98tgpYypJwIXZYdU4mJbr9Bbk%2BJU9aqTEULkoa7IggUiRAs7O9xaX0O2GVnOtJrSvTskn0t7%2BEvE9iGm%2BhM%2F2j7rfQgeRZPZ7XnpHYflGbqQ8CGGGIpzetJREwVlH3nbkWJJwkO2s4GGea%2F6pgmdV1lmqzXef1B5ZUpRSMF3ElcWBa35TxWjIzUM9TgB%2BaoFkX9dyi1aT8hMjZhh%2Bv2eeof%2FoSQPurXXtRmnFISWXGQKdGACv3Ba73c37joCQ%2B2qCQu%2F9hKvtcqSyHW%2FNdkItFxfuwPdOwb9nBDMCW%2BDY%2Fa8e%2FW9u1VDljnKrD7wrhu27iBkJnKeQbubzTAdayQ1INNOKZvDngvEdDJzaGs88BsiKlqFw25IiEF3dgZ5JlQLa%2FAj9gnWcETE3UfntVkqSfQtUADAty6xV95gqSSemwRAcPIbJW0wZhXr7eN9Oy12d9Z9336WPvwOhrxLldQQCTRsnnme%2FFMn9PUp5quK8rDwUeY%2FdazYgWvFv842dVFp7yw3V5uIK06nk9kUpmQ0PFBjKCQxK3e8UMNnNrMcGOqUB733TALHgNxZAUAsf%2BJzBHQ5l3hn4Z4UAjJ55YOLl2YAT%2BRoSePNfXQ60kj47sYKJjXJ4hcjw4eTW9zTYw5Rr3IRcB0dLN4q6wl2yugD55Ze92NZo1qXttqt%2Bsw1WQ8ceXTx2DcoUfMnuehGRRAcQAeUG93Q9IJE0emWJUYjDxFiB6Gc5AXiQ%2FCnc30WJN3vgyuLz1%2Bl49H4S0ueemVdP4YAN%2Bi8u&X-Amz-Signature=70ca0a660f65eec84a7dc146757ec80d37ec97b380f30a6203c62765981344e8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

