---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TN4AN4IC%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD7c3IWjRG0LjPmea7RZ7UsSTIGuiwPy%2FvESwSSQIL8vgIgM46sEsiDd%2FbSzWk7s1j8LIZ3e4LDjBbCPpaPXTXNfhsq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDN09j%2FbjXod7SLELircAxSlLR%2FO2717GMYRWnc53UP7DmF0ItL88W%2FJG6dfR5xfUe6%2FMOPNiBN6gkE7e7hQfmCnRsGILH6%2FsAE1QDbwu0Fiit2bNumEULgVZWpndmkDWR4ElTb76WS%2FOKla9EFkk3XXGqeNQrQqQk3NK4z10tWMM9cmI634UISMHaFrKSUO%2BFW%2FDNVM1yFbup1uTS3Kre1qWR%2FLtatB5srh5NoozEgLlVRtEBkJzIDXSjOU5mU12S8GZUz9xzGojeRnPC%2BRy8CuMXwYsa%2BfNvD5xZ1ptUlf%2Bm4DmlP4ZJ5MmgqIi8qB4dV%2BnMAGPL%2FmJLPn%2FP9hwIxIQKx0WiWD9T4HiyZ%2FFbHvqP9mxsrvBmA68OUvbqCO9sMNB3SJ9vlcIoJ57GWcKGeIlf56VpeWdqFIQruqhtaTcjWM%2FFLBKYq4FzYP7J9h8hToiojft6KNoZa73KKrsgSuu12OeJC8gP%2BYAcnZq%2BbaAXFPpodTpzWCyYG75WzsM%2BbWWHjLGFyjeV7QAE4UExWWgpgheqrKw%2F%2BSMhSzhNRwqP77tLcq9G0f%2BbTVz9IdICbgdH%2BhKCL2aNsmGygB5T7n5ibmJFx7blxvYnIW2onngas6dQ3gOF%2B7sQ9UkaXp5B7pRIVvOd7QKNLOMMmPiskGOqUBRDHgvkfEaMXhdafXjs%2BYgF238y4OK5yZzsea%2FhTfqOoW63dyBT0ALWVpJy421ReupxVy4Nf9IO%2Bk%2FRw0X%2FSq6V9ISczTIvNEcQZv7kNCOjUJ%2BBqoS6Ex1oLWJwxJ5bPXtH8WlIq3Nek3R0mP7KjWhpbPV0w7vRmy4XRSb5tlqDYKpbHPWcaJOQyz1IvVezdxOtOLyub5xcu4CsnOVpi5pEQ5Sjf9&X-Amz-Signature=a00913d7f945d48a30f58ba5b10ed5044b28b0477240481409699dfeffd6ad81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

