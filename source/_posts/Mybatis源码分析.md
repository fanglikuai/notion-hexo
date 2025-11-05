---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWJJUSI%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDT4aqlwVN8FRGH2rWYRJ%2Bwc6doyWQR6YqkMJZ16Gc6AAiB6HIA9Cyg6nxEd5XNijT%2BvjF8gtJHe7UUPqflWr0ne9CqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0cL9K%2BGltt2dx2BgKtwDOjtpbITm%2FGXpxWHirXiyGwty1Qf26Vl8GXfBhdsHAs%2FHl0VWhEsFoRETasllDZRSGFCDy82r3Tz8C0%2Bahm8fO9RjDCJlCtte%2FzKCIGzd4Ml7cZx85sY9p7Go7s1B1rsBI82juAWZsOFXhY52kfM9ntyEqtc9I39PscHK2MdCvHStgtEC1xw3HX8l2zxi6GuxL1gOsQWSemFUoDMSHgsR6K%2BwIsX9tgZ1Lyha2Ve0uAn8PVwKxuwpjscrD4E5BTsllQ0CJPpgrTUpjjlIVJ5ZhMDb6cjsxjV0ODo0wCIeOcbxH5YngYmV3OtcCX1AiiyIhf5huQrwHLPc3SxwDAMW88v7wi9M6t5BBbVfoLdYabdsyow8JB3O7icC05SCOdEBRZCSKdULodOcnkuBPnKP5%2BwgJQRUxPvoaa7%2FjBJRiXKED2XW%2B4koIFTfbtkttV%2FvmIEiMyYk9mAN93FsECQX01ujN1h7qxcxNdN5f0Avv7A%2BnnKriRC1oSP%2BWQF5qXw6vrXfJeiLUHzTotop2deH0j2pyY9uBEyDCKu4wqryQoHDRMW95uF%2FhHWe6beOiaiH3H%2FIRCusQAYxPEWBGXVuLpmgn5B7mmnbZNKsBUx3xhOe9jjRlUfcE9PsWBgwkfmqyAY6pgHeuTYfiRXtjfrBIP44cq%2BD%2B81%2BG80y8De3VEqbkw5W7VzMic31JwV1giF4MkNhXPDHyZLqJgDInxZaM93PwV83irUenQ%2Fa7MmcrTOpy2411LYXf1aFEbpfJI%2Fo6dJdYmDgWbOMlyZNyTCGKQNPZRrqe1pcOqha0Y0ectb7Lu2FGJ8%2BP0EeNJqfFpMzMqRMlMHXzNHPbBISFfpSJdSEh9RwftfdeVSO&X-Amz-Signature=1cb7b1f1ca4ba7bba1622781da79dbf80470a1ad634e0b2657c7fe857a62e2d8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

