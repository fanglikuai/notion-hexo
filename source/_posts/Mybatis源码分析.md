---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTZI2U5Z%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQc3dXl1qo%2Byij7hFBcJ7JrK54vENQeknUBD8AtZqPxgIhAKnFcGDon43SSum3rdhIIzVjq4I5O%2BlybestnPqfuHQFKogECKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysn0Kq6HFMZe1eWM8q3AMcyOZoG8m%2BpEsYCIIHsRZmEVTrpze6oFeDluIJ%2B2msJ02V5CfwEpqJ2xa9gQKc1AZn%2BIpG8Kmu%2FQWsuVd0gQKloxESyCeqQ171CljmpPH5wtjUcrCoHkUim%2FiVaH2PHZjWFrkFxhXrCCu0z6pyyY6kMSS5EKT2flZYUPW3zx2z7mfYSwePRR1PcfC8rHmyOezRoXkQ91vqvfS2%2FLWfuJTrs%2FITWmumC6eIAmioVlZCkFshlpr7eHvXCmKXBDs%2FjtHy59BXUpkwR5Vy7ExZu5gikhwNINn241T6KZfrzDh%2F7AhzCdWOsfW9tTzmSs%2FcbrIF7qIoY3WyJIyEeIYrqaeE%2B6ZXyi9tASNHi50RX7FOCstWaqhTAGS%2FUjS%2FN74v7xK%2Bbndn4gcxzAtnnXoVF%2BJUDsZWUTW8QfrtCJAzKlrthfkeXGc9O3k5BLC%2B0oK4WZ8sn4stlTlsr29wS0xD%2BW9xhBf%2Blts9QRlED6wZatDrTFwKqVfJ23YGUGsez46hkld4kyYB0EuQE3bQOIOBzMDp9MVS91%2FMXyhSYhUUFoOJLUflTw4gaGeYaa01ldf0sgizm1BSUuhDTTv7eK3xC9dz5xR9ru2i6vL6L6DA4QgVoOoPD8vfHwifNIvzIzC2ov7HBjqkAe7H2g51VpZEXEK9kU3u1ldlCkuEt9RSVCAxV%2Bh%2BYSCJMYGKE8pTadB6aiznor4i3ABubMdhY2%2BLs2iD7r%2BfjbB7GuNquiDUdT9x4jsXiYv27IZtQwpoMyrka2D7Rv2XO7TPePId0lT8WUL9n0CcwW3gqAFRSojTiyQnMyUBEeSL5uwK0A3%2FftOOQ2KHo530Xyz0SbbaI5G5Jx%2F30rRUuYIkUR4c&X-Amz-Signature=b87f90174114f4f914a42be77060eb7331ff8bb66c7f9eac3767a4e5be86a62e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

