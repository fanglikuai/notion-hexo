---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VLVLPKB%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTGmtGSrPKvJmnEnK4dPHi%2Bo0jRI3APRa9May6kcgNLQIhAKwmt7AJb3MQjN1m1WisteKRhIQ%2FZ83gxmXIg2w0OUMyKv8DCE8QABoMNjM3NDIzMTgzODA1IgyDmY44goKdN0K9guwq3APb5tO4TlSD8uUwJKABhbh0t4LbKrFJkhcJBKkMrRlzBVrC6FVqUlMVdfttOs%2FsH4EsuAkvapev4GBjS0VkG1rbMJ%2FVaPalWRNWjCD%2BANpCJz3jnMqLOcpYhPy5VFizm%2FpYraAeT%2BP1gNXBKsLY9%2B4arli%2Fk1ijPr9thuCyEtISEqe21jAdPPPPjEb6BVp5Ovw9zRTnuIvID1XUcXOwawe%2BfienDTf%2BfXyUIRz08TFRwzeiuQV%2FHfq%2Fx9ywsjLaLzSN2XqBM1ChTEZ1piIdGW3EvvcrOdZKt21aroeEELeNGUIdmDQjcQhp1U%2F3TmzkBNsPDkM9wCmsS4n54Nk8iRFjbPuwhNXHw7We3AtsTwkZ9tVqBUGT0u7zNLCBlMSsnqkR%2F3znKAflmaYIkzor0Kns2puzo%2BqAs8XgTnCgIWk0je%2FrxwYDLJfJMNrSoI7df1rcwm9GAbVB%2BKnDbySUCJKYiAQtlsvqY3B27F4gU5yX1w%2F5sjt6rYsfl%2BPQBBGumuLnxts%2BHihGHQP15zsdInRb92JATztRRO8TAYepg3d0wI%2F1%2FU8qWo3bxPWHxM2NKlFZXu7gaZRXN6vbvHMDc3A8D%2BPqCambLFsgB2%2BZS%2FTwpUzkEZ38LYD4dMlTaDC%2Fm5%2FIBjqkAW9d6c2vygn31fNdnBY6I9gL3iQeSLp3otyfRoGwkipsx%2FdUO5KZqIBulvKJ%2BKGhp1UH5liN1bbzN06Vc1VErB0v3rSzo1KYvzww407Ik5rVbfmtRiKN4eZsB2pqif1lriJYJi%2BC7AuzveHnoIoRUCDnwPhChyDglv6%2FUw1bnReGB2WwuB3WktdmAzWBFFEfdtgCubd12R3AJsUOM5NmjDF43Iz%2F&X-Amz-Signature=65beb533ff78d17410259f220d48844814b4f34e40819d5bd1edef156f7d068e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

