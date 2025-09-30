---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTDOINK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQC83WoyLZuTrMw%2BzdyX2o9J0vI%2Bi7nPPblGZZ8n1miSZgIgPnY6pwxoRynSCH%2FF4%2BBresUZk6i0mmKJfn7YtXw47VoqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeEG7yjkfVWDm5R3yrcA5KwFAH83j8PYjh%2Fk53zKIfKfh2b65BVLQZqRI%2BEc77modtwZxU4Rv004bx3paaU0YuK7jskUo1HWd%2FjwxXS%2F07pk3i%2Bgc9asdvkzG44wlh220AON3UI8eRs64lXsakucv%2FtG7uPVozc3QGveWtJxEwPPuQHDJs7hYcer9SnE6xXL8bVR%2Bl523J%2B2ViTlxUNccwOc4Kly8756D9cQvgkwICdMjK2emUsN%2B2mc2%2BkeQ7Jyf8oHu48Ie3ruIFDXw5CVWZ6OF5oCf1WPDE%2FLbdKml9I6qmGeHGBstridf2Svw7rYOWcRBjrVabkHWuM%2BJe5mAPa5wy%2FGJo8DaxIMD6RXSGH8gdjSQaxBhHUKU1HqIrLmhqigY%2Fe9wnjd0e5y011bZRGaqYHmFKYfwz6EM%2Bm3Lc7Cx0RvXbMQVnzLQMsY2f6yT4Aosbg%2BWWC%2B%2BxasHVpVeWt5vVRzBP%2FdXhwyrLEASnrEYYl9j73mkp6SyHTu9WeP8ia8Eu0KzJhqwnoWYkmSw6QKlNerBbAqVZEh8xv60S9ESEpN6BUJ1kHSDndmUAx0J45Y4ISwIqx9kC%2B8gWkU8B8WMxxNRU1U0feJaetRx3RwnnjYZYhgbXyk7kDRmKw4dENZHFopMKUWQE%2BMPzp7sYGOqUBGCJUsKiuwyg%2FlHJ1P8WS1dRoQqgeQ3exrZXshXAYYCm0Kv3sOba09Pw0ln1rIqdCq6QExedsvnJ8JPmzdzliUEbYXYjCwooMsSgBNKcGnwH8qXL1WFg98UnnanQ7voS6RFbhwXLAF3dTFMJaWVZ5MGIOcRtV8i5GEL4kIh1IHL8BoYhjijxm%2FWU25F%2FcgN3%2BGXzJuKxAWhfDHx3VmFROlI5Dexji&X-Amz-Signature=5182c7a4d95324c22dfc9f4a4bb8fbe3fe532469694af31041dfb5c5f3bc3997&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

