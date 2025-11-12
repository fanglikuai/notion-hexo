---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664O4AEQ52%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQCI4bwGC9ULNDmSaorFu9Bpiwf6BiR0N2J0co%2BEfj7YEQIhAOlT6wf1HoFhpN9CbIsJunudLwBOxvaBwLZbOY5OF3GaKv8DCDYQABoMNjM3NDIzMTgzODA1Igz64M%2FNse4MBUjKWFMq3AOxMXn6psZxVkJzxuF4vINzsUi8YzjNqzkckAXL1ycjRuNbkwqcOOO78jL0NZGkp7VjvY4RdhGw139qIbkpvssphTmj3ppXoROizZVyoDGHU7gh50vN1%2Fl041rhcmxFkn2ZuAv8HOnB7CHDJCzI13l4Dtrq08b%2Bx6CqQY%2FKIz%2BuCxVa5KEagWsYAkxAdc0qdPjVQMeef7m%2FIXG7Wdq5A5mPBW5eQfPZmBSJWv4cVS%2FhesgZGC0L3qj7JyTOlPPLpFlLkZQ0wPnh0gnCoDjVyyJ3SlEi5M6wrkLp6gVVLJIeOGq0zGcT%2FGGz0ktsTCAcvRZiauq3rR%2BRGdKWCA9FBwf%2BjaY4JF%2Bwigex1fAwP44cfuVeFzoLwQdB%2Fqi9QQTmOK4aC27NyAsm7ZkEMZaGnTSA7fo%2BkTFKwQA%2FrJJ7tH1SLr6BM2hsvFZp8byVNRV%2FWxqykW6gCRXEsOQP8BS%2FjoHSjBgXBJQlDNjFZPw5TyNjEJ7Fwignm9hvPjIypdrCWTuewbQYCsw56ITM%2B68ai%2F7lqGDAk%2FU1CdyomBr2gVQsQi5grvcZ84Sh88ffQAuq84Rr49XoIAkSz2Dj4QzMnY%2FVBNhwW%2FYoX8rm5GEVdzJWoyRs73lcW9frYkaixTCKgtLIBjqkAcl4gCufrs2VpQ7CBxMgMW9Nb0GfB5hxJpJ%2FFFzrmY8pRsbGspQBc4CewJR79YjVoxttlulRtb7cahR%2FHHXbbffnRnrXAeWaCv9KfTofwG%2BwpI6YKv4RlJ2H7n2xAk4CRnHIIFTI8ObLhodEk%2BP0qz%2B5atEkVy8oAgdqIzT7Wq8cYVy5XTExke4uXDMWfQF9ZDLC2W%2FMivPAxPM%2FCotC4JmeTRdK&X-Amz-Signature=8e20f86973c032ef0ba4b7f165752d86dd0248dc5c9b8592ba361962d8641662&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

