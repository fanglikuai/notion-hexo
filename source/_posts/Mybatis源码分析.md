---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI4W3J7B%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T060039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQClGm6Md5ydTwUVENuzCPmtsG6jmY6ZQtyPanX2A3SVngIhANrXuQBLS0EZh5Polh9%2FfURw4vi1wpYQFTMGWpPT7MFPKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyTtY3e0LE1ydwvpKsq3ANv%2FlUFYRgk5RHcuP%2F0aUzXtijUo6elvgEnNugllBaTW%2FgF20bPrPA30xON3fR76iLYoXDeG6LMwJGSVLrDlBmuIkmmPHOeKWmjjja5HCcyPTrTfGCWkdpSSywsyrLxztWDgzpo%2BmvJZxb28SHTGp8tEP8CgkJtscjSlndyek%2FYKeAmxHT6rop%2Fues4G6NaAct13ubmrb9isKMVUPA560an5QJ8%2BH%2BWJUsptVAo9D5CbX24EoGeq2smY2jCkmhdYkTrsaaAm3kvx1A5RI%2FAS1dj26GrEWJX1J26mmEpcR2AqvjSyvlUdhubzWnveTnyGfyuOOZ8FPXu%2Bq0soDk8KXE0yYmrWCm3StNeBhGdlUgOHL%2FKIc1NLrKe4t9kFXrd1UXrqX%2FO3I6zpiXDjtj5Brfoo3%2FKtaZpAaisXtQ%2B53SyjoumxBWHqEEF%2BdiasciVHnAmjayS0mJoNnQ53O0aTDehKb65FPDnM%2FYm1jXEHJD2UOv9jPR5j2zB%2BLv9TLF4gl%2B7%2Byp95q%2FO0hDP50iwBfr9l07XJgJ7pql%2FxwEKVaX50BGIdwvNWznrkyTFC8LTFykcZDSU7m4Hk%2BmKGzvwWe5pJiOW%2FKgpFWtZUsjwSr5iLOg2ERyRd0bdArsvGzC5urDIBjqkAfuoMGN%2BH4tpVp5%2BgMoBYEmFpIuWnj1Zqf8fZzxJ5u%2Ba1w599jS9ozRrvLfquBfycBtCCks1iTMosmZ2Nhvn8mQv4PwsoIwnNwn%2BfBbUpSB26AFZKktlhbDPqNyRlb4vqlMFvLAfbkapWtxLp0%2Fd4mB8WZ4mj0HGG%2F7Ilg5D4AR9pJy0Yg6kANXPnyzr0ZvZ9P60NLdKco%2BeSOjKHZFiB4hzHoCx&X-Amz-Signature=84c0a1488837161185745d18608766ebea3225fc7029055a6ffc29070f1ba822&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

