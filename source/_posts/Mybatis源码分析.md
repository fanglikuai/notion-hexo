---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PKKCW7C%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIGOob%2BJto1HWuYxxBYyLwI09xrsTZapFfBCe8FDvMYoyAiAPC2wC1vpYjhkutiCf1sicYI%2B0rA6ytsHGXviRzCLlTyqIBAi9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdMSzAkMc9XPv8U41KtwDmfJwn4jkiI4UuQo1Q%2FPCaoeC3MvDV%2F%2FY6M7RLL5GVop%2FaNeLRh7uieCAwudAFMjB%2BblBoUlI6fZhYKpsYipPX%2BEdPK%2FkdDz5AsBqV4n%2FG85aldHHc%2B2ksqEVf8PpazRENEiaXrTd344uf27J6uMN%2Fm9hWUYBvB2L3tKZeVdwlC7pykD7vf0Bp2mbstsUvZ6v4szis49sVdF65E0yRmMJYJkB1qSYyBxj%2Bw4JpbZ%2FUcqSlrwj3yKIyOWwHx0WSvAHjgWpeko2%2FiGppliO68KjCAmvKTcHB5uY%2Bn15r8nvIwt3ivEAMw26QfcXT1QJLM4VBy6Hr48X8pACQCDZKVzw1tpw48r2H8idQoFbZjV9%2BfwvrMoD%2FO0io%2FN8eWJDW6oKn2mEmpDRYx9gFRUwCneCnJFLOpnK8Dbr364b16QsBf17SXxB6bIergC2KbNngiFGdRyBcg2y%2B4%2Bs1UAxBpa%2FPa7frmK%2Fdz7Vx7UZQrRNFQKKY%2BBkIEn%2F2eRvCg8A48md%2Fmtr91ToW0RAN%2F9cNMt3xZyB0VjO0KaNbI22lZuLvnHqr5zC4rJGyQPD2OMMzkgZOZmxHZgsjDDo2Lr4OH71CVQrHwfINzTiXznBqnQWIxwZvB%2FfU1NiCy2oElQw67%2FkxgY6pgFPaonAmPNue0YL6rsRdeMVdiK2G7jstvkY0CJvGriu4TpqWu1ne%2FJjmMiB9ZgwI1ZAd%2FZ9Zu2F6KoWV9HZogJUq5Mz5J4dA1peDT%2BRVeI8D7SWzVu583YYHyUJTpwPPZDAKpg1blo%2FVgK3gQrGIXrDkdmz0udOGQoSvS90y3b0OuCnIWI0uEW07BjNALRptxtSkqdpOgIpseslIjw%2FCFQFI8t1gg74&X-Amz-Signature=40463482c70954ef8592bde8a86125e45148bd7508579e158c7c3fbf9856cca6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

