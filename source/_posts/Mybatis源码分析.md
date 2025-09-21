---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNYGCL55%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T100038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7GErKPnw8nvs3zWVAxZ%2B6KsEYjtRgv66GJRDSOBA%2FAwIgIkZQebgmteZkWBDK0jHRsEES1mFbpkemiz7ZM1av8EAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOV1TZxBbPsv%2FDhQCrcA5aOMZ%2BwTT9qxKRjsGcpASIbadr08n3LxwxXsf%2BW%2FoeRHAK4WEE4vZj6Ij5WbWW2OeJxJCBIaIeMbtTfTKo9nvikmwl6dkT74FCWnJ5yXCQ%2BJ%2Fnf2DPUypjfSvwYIbzn7xD9tWNEU5fOZHhgi4lcbPHwAhjs%2BzwYXJAhd%2FfcmdsPCCHzbpwigRuJM%2FHG7jOPhMxcBWfrn1WL3yHGWp4R1gKOZIZkzeZZTH3MY5Czpw7XTGxBKkLPGaluttrrCyU7oxtdR8Z9Dr69cR4dR0Ja2aWUz5%2FyYb5DnhZO5yOh%2FyIZ%2F7HBh3G5ItJf%2BBQQI6Ck0Gp1LNEWcEMUB%2FS3iB1rDo7i9NwmKqI%2FmjSxNn9EMCnK7xtn955S5WmPaMCAXdvCTWo8N3871hcnlEVovcKtfRDMiHucrxxvMzWPToIZyeVHDzFUyIyxrVmt5fe44mnN4SgGggYXLF7leT4PLWUAqjg8BEGvzS7Wvre8IOi90gEViaFIQRdO8gdu9hcxDG0p897om5BhG3qN5tImWUlGEOiX9O43m%2BZMin%2FOL%2FHCc5uwbMfWLYZmK7RrR8ococOzSw3%2F%2Bn5m9mjtZbJPgWw98pdH%2FrKOR20EJjWbcXLNiGYcy0KrLdil2plsDw1iMMb%2FvcYGOqUBZmQgQXAhvBeYhsJ0Bk%2FR2AzURv836R7rH7ieLzF7GMP4ESMP%2BAz9cTUdbxX8rHpJxVsmznOqrpDHrBHHqp%2BXJBsIinBoQiJCbyqvyRO2Ni6iejhwYBgCruhCcjiUv4h2JLxeouP70S%2FYZl9MxU6arFqorkDNy0bM6kCJRJpQJv4U%2FobAXCfqev%2BHzbmxD17qWo8LvvDsuSQAjZ4tLSTpsMGP87iW&X-Amz-Signature=dfa05df2ebfe37fd2a5c331224fbe29b2904ca2c674c43104ac146e007bfbc0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

