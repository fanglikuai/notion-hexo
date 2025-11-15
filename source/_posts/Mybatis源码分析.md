---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OHAF2LV%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEst0%2BtQM%2BS3fT8zJu26zsHb90EhZdnZXVpTGuMoUgSaAiBjret5OqdaASgqC1siMhh8uDxbJCEhSBiK9Ia8wm0sayr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMU4w3DjOsj5wQEMHyKtwDSDAOkQLircbIS38fG8TdHJEXMuZp1d4Y%2FIJTAqTZogjs5bv3Jyji5jJxg0NHr%2BZkWGMHQvJbneCE6wufqBHlxgiL%2F8EV5J3qL8PI4jn21qjrxhv86RkSLZYUXiRocWPDAON%2BPOqKkna7IkGW%2BLLuxl7qJceyrBOvMiR25JMibWhTOEeYpR39lEJsBTR5v7P%2BvFY1dRU1dCD%2F%2Bnt7sseb7mqzfyZlOxVP0zhFxYBJxeqvFi4dGFeDKoeMiv8UegTpa3druo6BDAtRrOawagsoq3NCZ5FBQJ72T48GLmHJ4dMIIoemyg2nnlR6%2BpOJGRK0%2Fic19H4%2BzfrjM2wNsVrddRzOYLsxtUdYdPPfxEO7agX%2Frv5qJHh%2BzdhWeoX4%2FcmbfwKB2tzE4WOE5R45YbA4WiplaLLGv44%2FARUwhVcaludieINXYD%2BhtHJhwA4LsMDjrBgamWc7iqdX0CCnqsJoXlUnhJTgHn7xQG34BfeXEMHFcXmrOfFFciGa00lIwc97QkeoF9QHN5dq0XXEQ4702m8fNjPdnr%2B5rxktbpVeA6ErRdGLk789Qsumj%2FeXAzaZBIjs5AjylZyeBQv4p2or7hgElvoaVpcH9O1c9pgXICSsuKQohsxRl9eZiuEwnszfyAY6pgEVfgGN6mbrK473ZUQZ7BL6%2Bnc9TAbX%2BDzw3jWkIuzK36w3vpp9A6LEKFHwjZnfMkxvbeL%2FllKELccPTt8vCS5PFK8z0HEOa%2F3TbQ7UJiaU3rajOIaYuvoDZQ5X1QhEw%2BtACX3vWp0ZDS2%2FDz0eOYpYvb4LmaY47Ko8MkTLNBF%2B7i8%2BZOthRUefXZoccYn4fftsH2m%2BV67%2Fl0GbeRAzLfZjRhdcliTJ&X-Amz-Signature=20b09b1463f627ce2dca90c54a87ecfd22e60d45b7331f50dc55449e573c1a36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

