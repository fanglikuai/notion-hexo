---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q65T7QQK%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T160054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEsRTfupXA2ToHU%2Byl3ezRbExnkrywn%2FU9u2ZmJ3CKeLAiEA9wIcJ5kHBiJdvUXEmtjt3B%2FluK%2BzgByM8D8eWckq0vwq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDHHqeDrkt6V4e0fuASrcA9AE0RaZ4FbcaIc3r%2F3Wpp8rWMd%2Fm8%2BDRYhIgsPz8q%2F8vq%2FtjvYZr0AaTiWfpLZ2EU6o%2BtrTKVso6cgmObJYlYEwn9cL4YxdNbP6R0c%2FcdA8VEHEUyqLQTkoBCOKXH9isd%2B7GnhWcBU7kgh1ipIQ%2FyOdvHGkvjGC8uB0aZgpJb%2FJ0GUeOcddjcucA56Y%2FQivkTByr7JmiBikIl1RHCUlLGVlJ0K0GmsFrs2zs%2F4%2FVechqa3kqQZXE0IHbOKceJGX8iVbVv03QlllIrVuQDjDW8sjO%2FfyAI%2FAHXp44s16EdWTeV4rO8%2Fe1sI0DLPTWvpv1W%2B0Sv6YjLeWFbD53%2FWYEJOhQYQ3J50P86315FvPPDaKu21FWkl1%2FZJzt4%2BMNx7IoBP2MnNX1LZm9ZwjHJaCrwkNhU3i6iu64ekgJGbH0dTZLPbxhuARX6QWe055aKkX6cluUctgvhNXjLQqd6aehAH5F615BWa48SeBP7WOS6yCKywOy3tYVlE6OXLk47h6dtjITqYIKjSSdrUD1f1FJd3of6W0jIxjj1oFl8jEXSgMasphxYVMXpjOpjHIvzZUZgzy%2FsJ3cgO5hsZz09hkb5kXSlbKOsNTdyW4abjxO%2BW6ON732U1FiGbSCgVpMNj6kckGOqUB2D9c5SGdDdF2te3I3ObsGZyN3Qdkk0mjRf0Caz6h32VKIVCnVrmFnIicRUurfFumTyyAN4nUKCvIJYYabN%2BIVf8Q21VW9g%2FObInT89DbPnwv8CL%2BWct9ViBvtLv6OWJMwWyb6FbKS2e99a7U1wVcYu%2Boj1N8Iol8tA6RoFucJTTkbR%2BTNu8q0Fwa3RiHbp%2FZcDpedsCarMIeuPzDq7xWYts3G1Or&X-Amz-Signature=7aa1127fa7f82fe0edc691e37d83b6fc631118e4ce78b3f368146d869d8fcce8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

