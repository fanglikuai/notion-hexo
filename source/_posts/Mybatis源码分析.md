---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KBPP67B%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBuMeTg5MeEXn9plOY%2FYFQaDdx0pZ0uJyxzQisvoYv4bAiAxjtMy6Owkhh4EAHWPkgdqS5PGwpP5hTP7TpATx0VabSr%2FAwg4EAAaDDYzNzQyMzE4MzgwNSIMwFN5mqLwFZV3puf7KtwDA7PziXQSagJCHE5AoFnQ3queak4nOl0L%2FPMEeP6fBH8XNipK8F9MKnojtPsvABiZH%2BVdAkNW4THB4vH0oK7zOM2rg%2FUveI9sfjnzGz4t4sSpL1ceqi%2B6kjwAfu7qHnXUTw%2BlTF618Qe7yQHYxRdW4XHMNyy5AlExGdd%2BTdzCoR3nUFMFDQWL6u6%2FlBxFXf5Sx93fbIIrNODecm2Ru8g2dpjC7uHcOJ3qUUNIibOhPNzgT8JsUFfGxCXFsrERbel%2FBJV7c7D3l6kL20b3YJqIoOIcWLHmNplLHQ3XaFdVOzwe48cZ36KW75KpGS2I6iKl2f6ulX%2FKIwMd3fBTNcuKTZ4id9CWKB1kv5%2BkoV0LbbB9kc6xLkSA2CYO31sYxBgfBB5UDJkiMf18Xxg2zUyeOrQCzvOvC89fdvLidGyAqYi32AxKN0bK4tT3f3Kyur0OSJAZIShd2732CKEZeIcWkSeyr6ic9NX%2FLrNjqo3UN5aka5OWvHciV2pND1ydy2KxtdkyAKzY0fAibL%2BBD%2Bx5ljdVDiWtysq23uc%2Bl6g7reF6ybeqsYSrcSvexCy7kuaKbto%2BQKTQnkMxfcyH9WsZvjJYNQVFGSVO2DULUdClI%2Fegdf6O9j%2B60t2N1ssw0eqwxwY6pgHsV3s5zN%2Bv31tirvRrF5uttKjzhlmKb%2B0RNiSVgAoK7x8ya4GNLMG3Il%2F%2Folu6v6rma8siyammhbllbZl5Rydg%2BsDRm%2BuiMCenC9LaTHfaRNmAgp%2FtwPJQlMNZy%2B%2Bo7hWGZmQu2I9k10qaA2TCl4kc0oepSPEyZH%2BbG6XAqZlyvOgDqxwrvH2qirV%2BXUVkpxcv4oJFjOvCwt3rxmrflhRMs%2BJOuGtX&X-Amz-Signature=a7f9b607d2b2164aa626efee3687868f94b0599e681a716f55a42eafceeeb59f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

