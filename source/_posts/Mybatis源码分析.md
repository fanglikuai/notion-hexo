---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZK3KKCKI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDhogvDb9c4JZpvfC%2BO%2BxcjYldhnV5EJ%2BoD%2FL7H5otzxgIgTWPHazrHH9oGKluUzUVXVl7JERubBg8JW4pbDr9ZeiUqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFlK5dF3bnE2OGbEOCrcAykwmBQSRvUXHm%2FVlY2c7%2BxJTOeQu06XeWiNRV1x2q1PwYv7ba4CeuZK%2FBPt1Kn%2BaIczz3x4twnE%2FMSl%2Byb4wSlXDfqoW%2FnG9b4udDwXdf5L4qD90Dw05IltuG2xcFSwzjJRNWnL6ZAsRznt%2Bgj82PDuZhtj%2Fby8lVuDa7VJR%2FbpTp%2F3rODmBv6H6WYCuChU1UGiu3RCzAdIKYVrpgLDnS9LRWvhX0JgQb1KUYOwtUMCDRy7p0Yo6152B6V9FEWAPzdTAmmGrcZbNu5BDxnoAGXF%2F7joW7iB9uPtx%2BaUJ0NBsvbUVMiNpNetUwnFuMwMklakOllhOq3dtUCwaExeXCGtmzzlOMzFHxNaUo8L%2Ffiu0c%2FgTvWPkrE6OMyefRIKGjKQZAFxVeqPg0xrYcUHGL91MbUOn16S1KSmn36%2B1rNa6CzvhIskoYo7aG%2F%2FUjp6%2Bnu5P4a7%2FQACoMEj%2BUoVoFr7LHWDiZ2FcLYcGNWPI5COr%2BixF2O9Ol1334lzl%2FQaai0vI48qSFgHoYyHcx1JJF%2BglgjnANzwlmBQp0lrxIT8XZ2Fdv%2BAzqX%2BRKlq9IMC1k7CEh4rA2GdnudtVrDvc2LCgy1KCy8islXedulj%2FYIFbPHkKYm3ViMUvvzlMKmOvMgGOqUB1eY8LwqojtUuBlf6vgFoZvPU6n8Sukb6cl4491dZU6qCICxgvEyAemzkCuow6vlC%2FJBagEVkgy7TSKdGRKJ1QgiLjqi5rKxZ7S24RZXRP21uubsWq39Kz6Bjum%2BbP8FKR7yS0YsAffmqEJa6uyak8Ni2iyDKtbVJD05gOJs6k9orfj3QhJEXV8xDisEgmXOgZEcV%2BJ799qzRv4Ar7RuNCEzYxQ02&X-Amz-Signature=e3d94ea9de32cb94e62c41bedfa21be00deb9014abb7194bad2d1d9e689eae3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

