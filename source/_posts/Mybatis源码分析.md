---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665B2KH5BW%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T110052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIDqFulw5y35m%2BKoJsQO7CHNKhtloZy41LRMeBs1jqxGRAiEAjpuPEssHCBs3TsJKDJ3odPrkdZXaBr9RzhPsYN4L9osqiAQIo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOndRjt%2BcSOT%2F3VMsCrcA1Mro5cs1OPb06%2BUkRZk1jyeBzcs0dcr83S5cr5CeHhzMUxqrsLWv9Ro5VFzfrFRnIzYWa5lKgeJ7uwRSqoBbAD7nznlOUhOSWk06kmKInmZtOW0hQs5fjaIhdltP%2FOXFPRVRK72c8wgXf82xtuIj5T2VqFBtX8Om2vNG7vKC11j6Lmsro2ZTs9NV2AvNpDyDrHeNThstS1M%2FnWvBS0FIc%2FVkqinFKpljtwnoUkmc5G%2Bk7qvZLr3r5WVax99QkU0NR2eVz4FNetW63f5gqU09HReHa7SCCiEN35GNUJ1aRiOIV5BZGmcBLdQcvUutwJBD3qfmQ4njaO7YdLNzY%2F4KriL3j4KfqYGV8X7%2BPCCWZUSOScO27ONP1xLUvR3Cr0Ee3GdG7xVNQBZhVONJioIR1chXHqoJWdY3D95b6NGIHEekEjvJxyXb4zuU1wtXP3vXPumToQVJBBIrsnPcNVkh1LMk80UFw0jcpG5kZbx5PgUBfNPiAxNOGMlwMEKu9uQRcVl9T%2F%2F%2BHnRKV1eL4%2FRWVeZyPX%2BvEuNCvA4weF9QwWzojnBO7aDnQzq0hIbhF3OG29LPYF%2FrQFiyiXO8YpXISSepcmE2szzylrPn3LiRnaFY7DGSrDZSkDclBaGMNKRqsYGOqUBKqbM6qlx1jMxZytB%2F6Z0GcmkdfKzH8S9j%2BbegW3JqtRuhM34Sh3prWfFA5YB7j3qLSvncoMz6tojTEs8TAOsBN%2BJ%2B4XMOwKGwF1v%2F%2FF%2FjyDWYFrg7PojRnIa1TSo8AhjB%2Bq39IcyS%2FkTKjIoUG9lCRoYiZb6RpOOiTwBx5uziVtqY5tRoiLf2jmNS1Jr83OvFf%2B%2BiDpklSzE2XrGRGR4AwHN24hc&X-Amz-Signature=d77b3ce7c42074c4fe1eb158b0544e8956fb3a27cc8ace103087b4c05ff63992&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

