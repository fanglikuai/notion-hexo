---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBJQPWG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIGioOW%2B7SYIOFsbDp33M3hyvgqkffsNUnMrcAhsOiCAHAiEA2Bwh0PnQQIWQCpkcH0%2FQiCNk1fZtNlhc5OBAh0o9cdoqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDXlK8flbNfg9YwM3yrcA7ETZB1q2mTeNZ2vpavlx69Lbt4D2wI%2BApHgU3B%2Ffv8684Kd2H5Birw2PCuQ5TlYkOAWAlnt40FZBHVEVOz%2BCs8AB5jnDVaOaj8dBewoNuKQjB1tWHCpAqd2wx5QL3FjQGrXsC8H35leabYqpVzd3qtEUpA%2FggbXkv%2Bd1bBM77f%2F0HFzfyO0uxgAOnC1vfZzVAXXCPex4l0h1laWWHOnVNjQCkWM7jo40DsVIZfFGE3eImvvP2X7oWPTEw%2FAPQWLoMob0iSWYaoVF5PPvIpKhWvE3iZ4gEK2wIzCg2H8GOAhVpRsKtk12xD7r7vD1S6SQ6N0ZsNQSh1SIvG7yh8Yi8suu2G61KPOeXrx9bnV97WjfBh%2F3gFLnNrnc%2F3egjQ9iwBm22x8PpZloQoczCsDjaPxsiEEYkbGTjsmbgvMRrLzEFyzybenyqEoNa0daQztXsXEjU%2B1VNPPmQCWDXl%2BqQSYHC7nrql00o3oSxTw6Gu4HGlqxKkC7OqiRdR7%2F6%2BFfYLXSLw7JocSxJ3PCXVydZu%2B0a1oWE4W8wFVUc3Rwo6UWhe1c%2BBj9ujvapJK9KrHYrf7bH9yF0dVnII5FjodrvMe%2BwmeP4%2B%2F7RM4MhV9LupIbnaZYOWdddzf9ZS%2FMKPip8cGOqUBxcVuKsp3LVJPXuVUIceYLGGfhK0GO6q9XhnTW%2BwseqvJOliMOh82s1T2ba%2FW5RaEyBFiX95z7ssdF%2F6Mm4bT9TYueMv2P7h%2BIMgIaLIZAvv8X9vM7kzXbRq9wMTdtfBzwAB0mumR%2B8H63DFsWpbO6SNN3VDWcc3uUyUcrujWI%2B5aSxRaOoTYxxHeajkIkjTrjLOwPWQ2k6YKjqXDJXnPfb4tsxMg&X-Amz-Signature=68330407f76d325038c33f85db4927e4ab8a764d8d7fa4b4068ee8533b35a9b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

