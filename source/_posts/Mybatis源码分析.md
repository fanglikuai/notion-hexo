---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX65JKQO%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJIMEYCIQDdxRbnQtkoaZcvfcHh9eAkvPoIpMT520z3eiURTa16wwIhANeacX2S%2BHfX5rA9XHy9X9jlojUj%2FurRIjV4MVFzB6g3Kv8DCBYQABoMNjM3NDIzMTgzODA1Igx%2B%2BJI9ZhrvRspRKCQq3AMUenTY5SjHgXwQywvxZqUZwNKo3iBTZZSAOPHBdlrQ4tzJoo8QgpFTSg5%2BdCXV9sp1VlAT0o1UhMlhwhzENWadzilpqKSunb6MYaPaZNWXVxzd%2BsoMKMu%2FtLc%2BPvfRhlNFMJHJgHHF5S%2BZ3OKb1SPiaLAyMSwhQepDdaVh94HrNiUy41pqp4hk%2BjBtfOSI%2BF0024cTMNiezWpJppItVWMpolJoabB2Ckn%2FYyS%2F6to2vDRuJR%2Fr9hEDRa%2FpyY3pfkwkycZWdDLGf5hH8Rs5GSyqXEJAqFZPE4SNzCReZ29LLsezMjD3myFM%2FdGXEvGG9D5JhmWbS5yOLOG47BzGtXuDd38J9u6N4WmtjgWbhOAg4EWF0LDKRvjOesaFYOwhZJXPiIyLZvi2VRB6jWzLCDMpnQlutKT1WSzzVuHs2MycufLyJSFpeH6msWpskm3Xw7g8zF0XbSDipCZgsLXD8vyjX0eX846HhRKxPx1Wi49dOGtP8p4mVHdCyHlP1TMWtt34IlSLJlnoz1eSIv9w9RoS843FyaLLQHhi0cpbKu5ZQadoHdhbiPIM5tqd0PIJ46nNX1YPDiSsvs%2BS0IU7mfBanwCikpeL0tZP0qZG5LVvduCtYGIJGsTXqBqkMTD%2BpKnHBjqkASch8kNwPaRPiTZ1tVxQD8ED120brzs5GkKAGkuapYXJZ700GJaR6YxsrMycqh950kygg428cFn8WP8DtRM6cQKrrl%2F6KeUY4HpFAeigbIrilwua3VsE5E6CK24rzZDVvPnXkZcAW6kmBv9iolZp55KtM3MCA%2FQlVsn1IjUtUKMTON02uAhSISxwulh%2BIxlmd8jcoDuiO3AHAYhQgcvRFjaHpg5T&X-Amz-Signature=a94ad8219704fb2c42c4d3bc9418d3cc4ed9472d46ef5b7c9df7c2830e02635f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

