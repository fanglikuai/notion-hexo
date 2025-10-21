---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667ZF6346%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T000049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIHo01tbP0qIplcDAlwVcoolzCFIuugzTK5C%2BBOCRkunFAiBDnbPjaKCap0RfxeD7EGPaG6K%2BjbrWj0v9wDEvrYwvCCqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsrnvyB7m4dZNJhVKtwD0HIT7qFULfICtocFzT3QT6vprfxg96F%2BWBL8HyBy6uVNgpI9nHVkvj7ENkD6LRuIFxqiXOrXw%2FioFkYLtThUlKPQoSIbbNkIzeQ5d95rqhjNp41FQPqnemR60Oi7tZmf1R1bdkBA%2BEjfRIdv9WJXdKFGCo9pI3N843vnGz%2Frauy8xjsBs3tR7GSMy%2BStWRyXSXyngdRdLcUqc%2FrgRZWbyMqUtvqxSZhWkNzEDFCLRwiqo78HQ85UTaCSGDb6p9K0DETXChKiT5%2FG%2FJ9SUTzp%2FoTLqQmmIxd0Zk6z0hIK541xIy%2BGD%2F%2BgYx5ZFbYIQkhfyUDAHHfv7vX%2BXOZdcSFhZIy6X1f8HEyMaskjqWmF9iNz2ystuN28GtXFM5mkvYHVPq%2FXW%2F5k5piqpgUlYrsDy9ctEjS4eRoFWcABrckphz2kdB2m5l2rvM5JNUeBbMfeCpgdoD9Vvr58Aj6NHxVZyyoO6EGE5Tq5J0M7J4or5VSEJBDJS2vOBGBAQ7OohA7AiyMGZQdzLHDoN4bzPBfbvACR6LBJv8duUuxWcfOPNDZKoUJamZb3LLUdz1oydlcorZrBUlxp1g7VnzOJKskmKhwrL5dlqsMi3wKBEYYSI0A8r1XroXmD4PfhIrAwgpfbxwY6pgEh16LwRNCnz4Vkt9XbsPTTEo6eCY7%2Fi5E3E4yLT%2FBu5EHQy6N3vKnZa6ypsSRFm4SpeoDIFZyFm18BAyzFpiDukh1p1cR3a8kbicgdv92e0Uqn%2B8ozO4sVwTwPdYzhXba4D%2BSMObgH7dIcDxrwSovjg77NNLwoPvDRFMj%2FuEbGNbccvPSCk7c3e%2BGsRKyRUAFRLeTkoj%2Bmob73Gh11nY9ahS7KYNKR&X-Amz-Signature=2c324860f0b03dbf5d53bb9e024ff135574978b42e6751393dfa464cfaaf9c7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

