---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHA3PO6T%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQC28xhc7CJkGALh%2FTzxuJ4tdZwsZ745huGl%2BNOMALq9WwIgSLo8PZzddqSUZpnaFzm4sUrVx2cgKGP6hYToULytY0IqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPv8YkaovwdYUdkpESrcA1%2BxH5iMSLfT8aIDeW8sWBPqRlP5QM7afUw8NrIcCRi9lDQfkI1vtu1wxS4veZsWC6OFGzDjHhbT%2B%2BlprctsAo3QLAgSB0YXiUCSLBGsCnBbbD0puPlUyE4uZf9iHRLdT8SN9XxBRiqH2cBoxuKrfwtZbplObe0VRyvzICXXYZqhQ%2BC980HLQB662TwxaacD76gmyNSg2rVyaC0N4OvCAGsaeG88m6UaxbJ0ClfBTSTIH%2FzvKirYSQ3gppeYAhaeEAeWfNuF9ixE2DdEvRiOzF%2Br2VowecnUQocSj3zCg3ZGhkICOuM2NAxvEp2aInMYNS5v1TrCgmAbDtoaTP5ognq6wOvWwK6P5olR50LY7wUQ0MwrgH63JQtLX0D%2FvFsYtJh8yTeS9QCI7jgoQcCGClizj4W4K2bSN3YtvkjUHbMVI7qLTe1ifkAfo57O1eQvJspMC5Kyyrl8QtSHj2%2BhUwqRXe7mVDVW4kxmHWU3cz0kjl30CvhTIjq3h3B%2BuWE4538gkCSe5Q0Whi%2FMHEKOZkinNol3Kh7wp9TUV%2BGmyJ%2FIfoc%2BJy%2BYLY6NO2erBjqqijUlRKz26SujgpA0h%2BJ4bJc9H8YQY%2BTwJtX3JzgMdfC3XhaJnY6AlL%2BBc9%2B3MILJz8cGOqUBcnirbiJqQRB8qk%2FfJnMp5Mdy5Abpz7YMzYK9Vs%2BrQoDtjXpM3NcLLPQPMIcuiNu8NC3LuACrtbrciqRWVua5ywbcHqJKPy7yYp8a6X5Lq48WJ2XTSg8elEQ5u0nMHkY2BTwQW7VjyG4LusvFp5MOjATkv69SDTxdUH%2FARdueUWpZRRnHscH40LPgG6%2BMLFrYvcvjOnsaOOb6kXDGFlnevE8MWnQi&X-Amz-Signature=bf2bc7cbfd56e6758bc7dcd12aef70de702c7e1514df327a823316c453997ceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

