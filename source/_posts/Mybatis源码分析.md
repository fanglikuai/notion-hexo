---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WH2QITQI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T080103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC7H%2BIwT5FGKC9HNwtzin6S1t%2BZ%2B4mPI7orrckE5g6G%2FwIgVduZoWjNk%2FDop%2Blyg1Bj382Ml6PbsUNmXebEtC7fXxIqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFIYPEIZAXhjeFl9wyrcA4gI2hs0kLDr8FR7dJcGOphASR36eN0%2FQtowK7%2B4e7bJgQu%2FB1UJV8aMTuPEUcFk0Xh8U2hdOBQDcerrnQY49pLRON0RARBbXGIfrAQg6tzF3B6cnXguh%2FfVSG%2BXnnw%2FoE%2F0DvQlTjPulEr5aN7z2BrJyvg3sCteul9Oo30YLorVq3npyid%2B3y3%2BAsiB04I%2BSmp4tevb2GZL0qCjtF8qoG%2BVAvnSM6kELu6yMOxHqKi5EnTkKZbg%2F8ABkALiAKIV06M09huNv7%2F39UBriK7L4ErMHoasUsWs1BtpxhPVoFzJvtEvjgIhkFzbJxvotVurD%2Fhj885xir1BdS8bVCJimb6XIZfv8zX4TYuEu4BPYOkxtHgNQWmjm%2FAThfJ2qKrUZZY3AUdZxBffLWR7O9ITt0JvEHIV9JSqUGFCgL3weU6%2FTm5MDJMfqj3KJVbhRKjDsrsvDC1cROzo816YXHEpyhw6e83E9r1xCP4rA8%2Ba4%2BLzxSqLeA3O%2BUTUFfyRRclmadPNbfqKHiAyZRQrPjyIVmr7q%2FFIwZoV840BUbIVWFF0dsswh4zUytR5UF8RtMBAlBPfa6566uEJjv%2BIWaauAbD%2Fe%2BiRfnfaBxlFbbpaiT9%2Bmwx3Nim9PKj7kHT0MJmx%2FMcGOqUBkIutZk2AU%2BYfF97Z7GUcXtaVRWqrxEGMuRaJM5v8kS6G5WcvkGikay9GUrBPTOxqBunZnu%2FicnMjXgqJjShqpfxwfJyljEKCZIdNJLUqPQzQ4V%2BC2bVgS7hh6VtZjkSkIt4x905UKt3pcJWOyqUx%2FkHdxDbx9szh4%2BmL7MhyyfwRsFM0hucO9SBnJ8IQ5cDsMAOrnxWGHt7URNp2MVJTWVZ%2BDKDO&X-Amz-Signature=670ac015c91afc63dc7958dea33be66015e1df19a669a104793cfcd669110dc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

