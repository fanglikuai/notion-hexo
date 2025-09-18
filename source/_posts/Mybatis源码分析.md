---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAGJTY2W%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDJhoKLWYG2ltvaIsrgoQt4bftJaIWWnaJkPd018bYB%2FgIhAOCQ%2FjQqONTY2d65x%2FI3c%2BA1kYWJO8hm%2BJB9D6lqsJI3KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwEb%2Fg%2Fk%2FAQX6Qg0T0q3AOcW5IdlGl%2F3fOOkqVeROG6xXmT3R5YJn4jdf95gvJhf3s0CLMTSM5x98bJ2UELQYp%2BtcGYb8mHjDC9F1zD5EN9l1pBfXrhx9hcFw5OX25OrpuBdwToe9YtaaWNBozAM32ZxwKpkpXMjHK42Gtq55YPyeTkML0q%2FPniRqCuK8XFfiUwIXqMPxn7uR7ia7kYhc0gzsUzChXcBAke7VF0MVlI5W55cFqRT0w1dWdY%2Bq%2FlzXj2HC2%2BU2DJ9%2FmzcB9auvD0HpyQko6E5Zu8ccb%2Bucayp5yz4kFH2EkTFRbUFSyVZlf7acfOeCXFGwDRFX6x5ztw6FbDD7mxRjY1GfhCjEqhaHfuScZ5dO86soOt5g0LGPBmaymcqzgbMETc2V03MfDzJQROf2nW0Vhycw7Y5OOdTVP8il%2Bf7mT%2B2zvhb5lD7QbN0nNdIBjtjmOd0nivXij2t%2Bvg87l%2Fl0lIXpPv2divK3S6kkDzZez9p1z6%2BtIGg6uRYwXRUhtCb8cE%2F4EKQ%2FkREqr6uWtx6zdGyNu6nbnjDOz9xqM5mTBEVBMX8tJ3eOx%2FO70J5oJSe%2B9PxJTy6nEVnSHz0RGZ%2BJUFj2OosYyFIgecvVph6gniyuswtJkVnZyHeT0sn%2FO91%2F84eDCgnLLGBjqkAWlxToYSnNJXO2oF8DTEy9aWcowillFYQ4eaxJTribWGwPSfG4nGLlxVigTaHOIg512aQezng0Z9GXRfslM%2FwnUHHsCRnp74DtTDj4sfwitW3UdlGB2pzfk3cETJnK2EuOVAHmeQk4hbQw70MispW0gQkF4hyA3Dw7AQBpDYOTueYo98F9bzJm0GQZi7xOi5WAUgdeuQCfLSnjE0YfsEsfOdrip%2F&X-Amz-Signature=fb3b65c8ce166bed8b01bf1597b18d7e8231fc314632ff5de018fd046c1a878d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

