---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OXK7GCW%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGEh3EgG38U4QTD1kXM8F7f%2Bn4F0p%2FV1SDWJ%2FrSW8b8tAiEA7Ebc5FXHMIKaGmodcJxP6KCeE%2BUs5RpbE06ihsnjFfoq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDDukwRLtWDODMtpWmircA9dzgzXsqDMgLuvLduMye2jAHVgaOdFLakoQtBy9LttypG5uZZrRVpgZgXrva9JfMKj1fU18ghTd7LI55T9oyzVu1cxGDSdpT5MqETwkYms0GwUhI0zpOxuCstkCbB4nBGhuFYHa1eLwHIcoS%2BKNupWSwgEZmntW9tCvf06n32UUKEbtjKXyybq86qLdIR4d%2F0zZRJhvjFtb17zlBG563yCM%2BfhV5TQRi9XsK4QYnr8VuvEuyx4PJq1TZesdDPNcqNn2SY%2BsnUXhjYfgJVtIPJoHFlBBEYd2yjYtqDalVhrJn2KEYjtksI%2FUYxSzQDphjbeaVQd%2Fg8MOnPhy3pLOQZ4NXgOmDoTuuF%2F1SVHdcCwW4APfUQuRJ97sYoaAbgc9C9K0js7nK%2B6yQlsbVWiqe6o3WTybz8mSsgFClXqNU4NfBy%2BB0yuQfjSJ%2BsngpteKPOM1nt%2BU%2B0tfLDtiZU0%2BioTXVq5cvM1z4krTHIFv7iu3pr4rSqRAhovKEM%2BPz0tmu9l7Eai%2B9EkM0t18KVH1sZAlDBSOYMqd2QmReIkNOxMan4PG61WeDOf8UhpikXDfzQb6RDy%2BkBT4R4vnWj%2B40P2GTQQ1JxH46LHgBwbX4zXUOJAKSQpBKx42yr34MLubg8cGOqUBb4kLrczwwl7dbPMlrOy3pHQVyk%2FtbYGOLAc8Cotgd3BEwmsiE4N4U64b6zYqjj%2Bv94vGbTk74SqNkrbmvXfaRzLrKMidBaAUNh%2BCQ44FB2WDzxrkgCUPT9J%2FQU3RPFvnne2lHZdo4nrQsn0I5s0wQnd80hQD9o2Z9yPkVh9UFu9vl9ESSfHiTvgbUvvYjWWWrd9OgmYOh9ZDNM7Q3fygTFxtqbEB&X-Amz-Signature=e0c5618287b0ab762da03b5a8913fac726ec38e20e753c636de0a8d482ff7244&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

