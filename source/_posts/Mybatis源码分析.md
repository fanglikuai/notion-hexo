---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6WIGS5C%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJHMEUCIHq6Th7Rm5WMONTb0aaBvhzOm0S7tlR2B12h%2FNZ5ytCjAiEA2%2BWxHnDaXuvzjPmPRqj6Uk%2F9Hq%2BtC3H4BVUZgONE7FAqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAxpj%2FHkWSdLP%2FUo8CrcA9EJulhswW%2FkPAThNUYNNMsh2j2CL1BhGHEdH3k3zf6aGpd7B0xUT4WcY1sYOmned2yY377V%2B4oljs2DDnbrM5GA9ymJLKyYz%2FTV9WjljAZeCooFLJHS%2BSOgcowAdvrTv9sTlzTH0jZUdHeitoM5VX2wFZnwqGOA31d8nxrqf3GcyAvQPsgV9Wr4oqID2ojksZRImcUK%2BjZoMOZneYgvb0FdXoVWCYpYx4s5mHvc6d%2BRgn3g6pHUj1cIu4TqgIlX6zO%2BJuGlrc1H0Yz6KyV7CdZq3TqWwGkfIaH7D32HVFu3Wq%2FZoQD7XtQ7KWz0BTqQ%2BsZI64Vi2hgEXEC3uGilVo96%2B6bNY2sI4r1WGwAeckN3MRWNfUwJ1uyiabgw8WfHwiG21SUEFXvAEfpji5AJ54jUuUCqLmnuKF%2FiohYTEjO%2BfxQuYkRX1To1fXlHNf6fHpUWhuStmcNzyirFm2C%2BAYmW38xSERh05JccD7q2UZ473ODTWOmiRhCZTc01dMhHkjq%2B0X2putWwPFvBXAd3z4CeFsiaGKoTbOcfwB6tdalHytuLFlcqQwFvOfOcevsY6kXH5wiaNUi6uQQHxrFdtkOj2WdvRm4ycK4VuiY%2B3x2pRqWCJOwyzkrENcf7MKiAm8cGOqUBCQm%2Ff7ap5hPxok5GNkHd5WlS2hX63v80t4KfP7kMqtb6DedpzoLpu3Cwt5y6b2nVtHvCcnOs8kqqCTu7vGOCsgOahKAiI5rPLNKRghOUU2NHcRxjPwGK4maUpz3DsfuKLU8H3cbnM4cPslhIVjPeAI8p%2BZPaeDjZruJOp3N6a4OQAdKeVF0OEdIKGo2BO2fbnxSmKvWBWEM%2B8AfsgKLywU2r3UY6&X-Amz-Signature=59f742350b3f6a7836524d9bfb5c75f3e3a08dc3c79ca891d6a2d8081ee5d2d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

