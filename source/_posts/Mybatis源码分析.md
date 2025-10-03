---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYC4B6GI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFlC0KXWScsf2Ze67Eph7tro%2BQc9v9A16eML2xBDolciAiEA3nhT5VCvVNX0SQYgR42bAnQNe6gLeaTLj5vsa1bSfmwq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDBgwCWT%2FZ%2F%2FKzv2GSircAwQm5jaaYWkFtOnahW9kGcxQPBwIoXsC8wnwVn1ISj6bHIKkqXt6QXbsYsGf7YUYM2WlBteO9My%2FjV%2F%2FP38jWLMR7sW4pyJaRM7cvT%2FZdG3hZvx5crOpwelETw9LYorNgCqFuafXUayDNPlF89uOy6%2ByWlh1mCASMxw%2F3uuahS159xSyragckKWd8AHoPnmsudhWUo5880jlW5zCaWoawQyUiz561%2FCX733tPz%2BvT%2BE0xW7EVixoYoKlVDZCFSyubok74YyhjvM6M8ZNl9Fz6hs%2FAP7BNiUprNBWir1bEg7%2BVyoJCKb%2BWx9PItctdUp4aaq%2FX%2FW3syq7uXpq0ME5pPfnCScB4rD3%2Fn3lIvayjtkbB6GZiciG08LzStnx4GM3xKcXhZj62WTKmG8EFpk5FQoS8nGNYBqgW5s7Zwb6YR6lkDVysiUL7RH33h7tOIH%2BEnjwFq4GsMG0BYy7rNWe%2BncDAFZp2behldogc0TeQpvu1%2B6Mk3imFJIqVNMVwa%2FUJpEKuPX6ubjT0I%2FgAaISkMuwxy%2BWwe1GF8kE6jhdZYaNZTxFVNIY%2FFcLPBScGoFLADrGd9jSn1h9PkxgOD45MbWykVVeW3jb5SlO1NC69LBcAnplksJ0GGb7wdt7MP6H%2FsYGOqUB4OdiRArhAKksy1f1meP0ytJ19HzhcCPaM0cc48PchZDsgGSn2%2FLSMR4Pr8aaZX6vArr2pmQ4hq2RbCHkKDK2MCYB1rhSGFCFeuBdJPO8Os3HazvYIUXXr7I8%2BUX1lYX%2FN%2FP0ACvBnQDwfVJ4Skb2%2FdU5Fe2C%2BaTk71nrHhfZN0HBJqTcjeInkUo2jlt1DEeyw1L8yEYfVikxBt0IpCYA6zVdZPhB&X-Amz-Signature=e107b1a37150c72d7e6533dfea6d8b3d819570bd23bcaba11d2a1f5aa059e616&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

