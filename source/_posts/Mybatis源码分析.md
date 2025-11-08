---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GVASNLJ%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIBCGp5%2Fh1yZgrG3791nGRIggONhS075DJj7st3Tuz36gAiBuhLJOcSaXgfcX9Tr9vFyDzict9HNXHq2%2FNSPKCulZ2yqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6gXdJbZr1kjseWlYKtwDxQSAOh2ZuHJgRbEYCXiSZB6Pm%2B0S1AevMrBtpLJvAkEXkRexuFbAFAdmhT9e3KG083RmrvS%2Fy33M1FgODJwcvuY20fP9bcew2V8ckvlBAgwGdMLLyZriD01K805ZKIOhoPP%2BjJTiD0G2ceoQh0%2BndKy0qUNi7gLOv%2BJxS%2Fl8cQvbxtg9RzuUgVCw1AbcKgvKwcykEzDM6k48W6hfC%2F%2FVfJKbdI5MqElbFuaVXKi0Thj4Thm9JjJwx0VYOPL5jLZwalIvL8o9LQqSvYlul1DEVAZZS%2BpDNA0QbygHmOd0xZI3f8fWPC3H1fmrT7UOi5l7LDt7UGQzcnX8g2bYJK5Do6qpkjuZaGwXVufFK9oWqf75kULRVB25Pwhmj9qX3SViHs8ft1jDoNdKMQHnAVeWeL%2FjEGfafbfc3mDBQZpwyVVYXwneRRIU4GSPy0S0iYAJxvCko1AoasacyHmX6vQfOOCNFP%2BHNWA2zbQA5LAfDbUnuGJEuXn7LtLfDoBai%2FDWn8UIRzrvKGGpx90HuPFTc0M2NAWSH4r1%2FwunaPPKd4tkWLRpvidFdT8Z%2BQhWpGuRDP43eMUG3MOBjISf1uzJe9J1347YlN5Wa5vIDVBD14g%2FCBqIZwLNUz8mLJ8w8sy7yAY6pgF%2FNEuxEXm%2FtMcPxQP3KL1rSPZx0SkR04MyAOeXE40fKL95t3v89x7TTDslNmpiWDFLynuFq4kPd0NjRvpFEbiEnHB2%2FKeSXvoVXfO2Rg5C9DwoJcBkQzCXgBCjgyUE6JuP6%2FmPrWwBqpxb%2F%2FWtacYqY0YVMNOeJCOR41kug7X8F1y00YigbjH%2F8C5xw33eeGo9%2B%2B1PQ2afp3IpRm2zCyzbE3usmqic&X-Amz-Signature=5602058e5e7e833b6dffc609a4cb3bf9be131135181e0d6e0cccaf213266427a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

