---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVQD262F%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDz9pO0amwxpZD0ErKlXdSufwFz3GkWxwXkd5f%2FGYw0eQIhAJsrJE3HiWFnyLfMz41XzJLZ980YN9xW2T1wWXVnbjVCKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBngntsbF7JJMNbHQq3APctIwj2L%2F%2BUCktbRO4tQqzjrKP9t0bhPPGjS6YJs%2F77PObf%2BrLnuoV5RsZaZ%2FH%2BV9DWRxUPrfpywH%2Bd5G4wghE6lw%2FX4L1ffJq1AjdIQf23JSw8QXbfSa%2FbyFqm3ah%2B64Ed0gcwb9YAX32TJ6d8KIhjdF220sYY9a1ukLs9XoK%2B%2FoEW77pcCbBeSt0IjBz%2FGxrmJfAjIDmhOGweYJewfwYodTJ%2BQrqsMSPhBakxzkLzSIMJFOgHhWd09MHRR40ncobH7sBJPIjHCRO7LxtRNnWna%2FbDi9rliM4whmOdC%2BA6qlMTBysBxunVQrKRa5oGfcsevmU%2F6iME1FCT93mSvSRBHtRaMhzItgCB1dR80AH%2F%2BDnmVZ2vSZuA29iJSWVLsgVckqgdBzFb52WFleMyv%2B24k51LPuP3qZlcPHOhSg9plkoWHYTsma%2F01rN3UYs4LqaZWqzf2zqe3cGjiJlz%2Be9mbgRIaXQquCDkVty1NpfH9keERGdaErqMk%2Fk%2BtuxHGeXQChSRf59iFn%2B2xAUqvI21Yp8Q5fFcYmmyH%2BroYUkeOAVBmqb6sgVSCgW%2FZHSCqHRdmZ2LKi1XGy%2FwmsTY2tYc1jZnD%2FyJVXjMFHx3cZmJgMm39cV7twmvqG3KzDcuJ7JBjqkAXgypXtXSdSZ1t6DKVSCeK%2FPNP0JVwenBAkxRbtLxu9J2u0AzWIQy7n5XzOz8RHLryYeg%2Brc3eJ8BHtqLwHpbR44a2PMbvjZ81cloR%2FX9esnXqaGQHoEnYdJwqW%2BCHRvCKW1yFJLkWZPPmIVP7pv%2FciDuCDT2BVl2YHvSrvUWShjBJTGh7f8NTluJiZoYxh%2FAFN8YJj79M9TxWl3IKbwfv4HtWdC&X-Amz-Signature=591e63de67511dc406e6879a37665a19f2ed57f61a2ac5db7ca47f5321a19ca8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

