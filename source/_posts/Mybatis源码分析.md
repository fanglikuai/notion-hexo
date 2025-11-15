---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KCKVFZ2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXN909XXCdUfvMSFzhQgIIntae6BO%2BYtQUHzzzDcRiNQIgU%2B4wM3q768qNKyuGk8m3KdG6zNCmnUsW%2F1idysiC1v8qiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDr%2FrqdR2r%2F1QFoyPCrcA48faWxo3CShfg73cI1F4J%2FBLhKJHZCYgFKFp4awkFXLkwS7k8X1NFFpbcrDVOXuq%2BF4e1bRlMyulzP%2Fs4tWvNhh78ya1hSbYW9Im9iEjGrX6PDje3%2Fez%2BwMpoGYAlk4of3Kh5sye%2FrTwM6V0rcPSh2YnsC25XuZbRfzV23mUugI7IbKfnQnNrpXQ2XNA0%2FujdI1JwOsXvw7RX%2B%2Fh3Vx3swTluwSoJ1S9KWGp7kK55lg7w4iU8u%2BgRgtATWh9RCyqzmj0%2FHFcPUWRrkRHJ1gCJz1xBM8MAnIaSf5Fi53wpr6H2NPYddvXgBDrhm4Edc%2FiBOWQIDSlu883BAYrRlIUVDqF8OvwTd7rf9wpZvlnC1QUxnGoFkQGsb%2FOUwtYSCkiHTnFuiw4DWMkH2vWl04JLcH6JoAdxG9R9VePgEw0hcAdV0QXDsGvs9ZxzQtxu0WOAkvLgtPpfZKPJN99DtQ57wwv8SYP2KGvPz0Zc97ISgfL6rZP6r2FonU7DIECJYj9Ht2cGvcj0GzyiCcAjHceIYtXGSWo6OH%2BxAc2eb%2BgDgJnQeF6eNOFNBHC0RdlnzOqJZchdyyiR374oWrpCXGrtYsaZJS%2FInaUWdynuQDs%2BU2LhCfvU5b11Kg4fw4MMyh4sgGOqUBJ95qP3bsMShWDRot0Xyurj0WVc4eNEsFarS5h6bfgBtyJEuavlvXATFfjlEi9s0CTOJAZwawMrf7xD7kOQTWnLE6QPo8EeqXwsI7qYr0vH4xgkf5Zdo2FAn7o0VfkKQ4fSNnrWQxKdyERJ96KigS7jEI5irhCM8IkDuRJKe0tbomrEffbOs57nNaS26gCrVrMsn6CHaxXXTDk%2BWakAjQwZGExcXG&X-Amz-Signature=4f4df25a8a99f01006ba426f1ff131e45b783d881fb2f075b36240639017b7a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

