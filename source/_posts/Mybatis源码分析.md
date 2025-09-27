---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667XEPYMW%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJHMEUCIGtn6Duf%2F%2BhySN9tue7wr1S4iAnfHLDcOjcUntVM6y36AiEAk7F3kyIwSIzswaO7SYjIDJl4J57Znf%2FdqeVHjkpXl1wqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL0upvA8ATrUqBOFHircAypEzIQSSSEXY2o08esADAjnneJoImaM7jM%2FlegjrY2B%2BZT%2BVZnY4gtDlBtIHS11kgYL42EGTvW76BQvAJf2PL3bGQMqJElPDTYxx6BPG3ZCWvjqrxtnmolzRCa2X2HG0BCZ%2Fp7PZ89BZO99%2BgDZHGQBhmJzlGf0%2B3bg8PS5uXUffxB5taoxPHoYpjlXesoK1USpSod5q9x3RTqEcIkkbaJLW1ca86m%2B8rm%2BLUg04nchMRpNKOT0D46UABlbhPv9tn72rZc2Jz9n2D7kQLwlRnrr1m2uEgcFZWwCgXC2yKQ2yANEkXnfR5Ap%2FG%2B3vchbNbbGxhl%2BjcMVv11XALHzUp%2FXgF0e83BlWlVdFsb3YMf3KqZnbNCVsQr%2FaTsa2HQKlNOX6guv9A%2FFCsgl0YzhBXKqDNxBR20obhaS62RV8gmXJXxqqGMBrZLN3Sn8hOg%2FBs74v1j2p%2Fn5VMOhZ3fbAqiZgGJPD6eO73bs24yebeFuhcrloKH%2FzeQ7pHdYCt5sNCRmZ%2BYhcgilMPDwismo%2FZ%2B57Wxlp6FcJ09CNwNm0plfYFQ%2FuZRoU2MQf8TuIdyLvXWzxppybV%2BpjQXsLgFVxy9mRpcEgSBU2a%2Bk%2BcHkiocbCPq5J69A%2FiAqWkWCMPjn4MYGOqUBB9gkrxnq1%2BHzZYQhzE9lq220%2FyS7LxmiyqyVsAkYDDph3aBZg%2BfXtsN8M4ML2KF2aGUZ3u12E0wfNs9paZv2JS7v5XHi%2F2Qj6NMg9RKfWfjHntzk2b2zLfJmzu4fwkB9wq%2F0A%2BF40%2FPIV3I9UJub8VsjIFNa3a5PRv1t8TbhtN9w9Iy8rIoPdWGY88ty%2FAWio7Ie5svGOLCocSUVTiZSzwSogpz2&X-Amz-Signature=0d9eb42010d15d2e47ba5b9dc09eeab2146302829c6c002a2246f99274fa2e81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

