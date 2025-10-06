---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GC52MUV%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T000035Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQu8Jg%2FlXgAz%2FUoZ%2BuxPiPixWlnmw2oAKOzWwhFbyypQIgWPpONnb8ibodOqfy3WJMZkO1OvTsRUJBfMcfHVoIY4IqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh8GXFaMbDic%2BepKCrcAxjTgYgOgEout4O6AcB8FpdsRotfZbqIgAWRp%2FPfW98q0l8yfM0JLVe%2BUImyfPkYJCBMilnaoAJMHdrRBNXtGvGmLlh0JQcPJgukkg27CdChKpKqhoAMDE%2Fpjswij5TPmXp%2B6VLdQffIG%2B%2Fz7Nok27SmzlF7F6NwLEZ33f0ZBZNDeDEliMbnVHbWz85V7TX8v6ZExQjIUWEP7Y7BbDxLM17KN6A37k2W4u%2BffNio4oHgrmHzVYMwvX6bT4cFfbEOAYvBreQYh9Ps%2By%2BFx5p7y4dekj0esVsHJPuDw54ZEQxH2nsJz4T2J0UzAz18TzaEq4iveUVPTCcK%2Bmyc7LwYZfBcrYhoU%2BFa0O4DI7oTdaQX9gUUOi1CaHUHB7yn%2Bl9vreUAWVlwslBDkSoqZP5AgZQ9OZemB9VnUI3y4aYbUEt2eqrkPyqDmGvkKZtWC7TaXNbhvpbgvqd7cVNkMIGk9TVaL1qX5heh5zgq8sKK3Bm51uqcMhH21jprXRtTdfAhhA130KLjHNqBgCkdT5qsM0lkF6XIzCXM%2F6XOTyLRl4tfZXUT2%2FmyE%2BFJpadL66%2FOvIhNVb1QYrxCOKzJR3xo1yD6cYcfZcrzV11muvequqVsxtdu3v5I5aWDMcLbMIb%2Fi8cGOqUBxizIpSa3YNP3n7dqJVV7xTWdOPcGmBWA%2FZHAPFRARDROaZvx%2Fo%2FfXUH87LwZZmyxwp7qSAVXWTpG6zBNv7p67UCBENTzL78INTEGQH73vaZFGd22nRdG57%2FDD12zxYxDXHH1uJeNmft4z%2FcvFf9i6O5Hq0jg3rBarYJVBen3P0jKK4ixSaztQLQanWG5d8MJqeu4YLi5N9ZT9juLJH%2FoS4%2B8AFBJ&X-Amz-Signature=1598b9f6087b61aa4dd7f3606fc8c48308aad0a803973a034b0a0ac717cefae2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

