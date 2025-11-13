---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666X2IJH7U%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIArbjS1RJv7O8bSL7WqYWgoi7PxapY7Hlkv9SAg35PEdAiEAgm8fI%2FOuwC6RcIRw9jAhs3VXu4Q5BDhQGXfWEsWsnbkq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDA46e%2FMcqW9GZ%2FCaLircA5PoQVn8JyXIHZev9bjW8D6K63lULLorOrBubxx90R4HHyuEwzLtXgmHNNPXROsRYovcJLe87OO67aT0xes%2BmZHOzt1KDPiyLKe0UJK3PtfDoqCFsh%2BdcfOwhU%2B00eYqOf1p%2BSVmwOuZ8f5AumGK65oJNnuIDhOtSIm1boxKLJ%2BI1JBa6REemZu2KqJElIc4JA2CGtc%2FHVwHmrAx7xUy8r8ih5UQhT%2FfNb0WWYuZqsclkwYIzXmYxSVlGxCaur9NSmMK%2Bpgw1%2FYh%2FuVaMmzJKnKPtF8jxwNIwAXXGgSD%2F2JF%2BM6MyhZVYE0QCGcNs8V8EsEGHHG0QqIsIEGAdto1sB%2BkMVxRLAoTJ02CIMCc8vjmy%2B0OOIooSSAkbOiYKBe1nqFQgre4d9cSq9tzLDnJ2o2EXmmnlabYFr2zlZNVTlNTydrTgTid0kVNyIZoD2hpGOrGvmeZE8tObql5iWGeJNWZTPf2ZrTRtY66WBJGIq4hukBbL46KLQA%2Fpbj4jfAPlPYJ%2FHUBiE3vG2kpEetvvoqjjeqvByg9C8a5etnT%2BSizfphG2ob6vE9PASHSHl6lLga8h812675iygydMo%2Bu1VmCReDmb98IeHSTElpBLkXp28C%2Fn%2BAYnqqbEpCKMODQ2MgGOqUBYNk3d3NQxkOYEPAmDMRR5wSNCy21ahLlnMMR3vXl%2F5CNGr0XtS2SoPF6J04P0pXUtZ3wjICOdBvgetxh2o%2BtaO5Rg3jFacsW461WC%2BLTojV6L0EuPzlf0m4O4TX8gLX1M5lqGoMuTbEsaZYSvY%2BqJ%2FHxNW%2FJb%2B8ofmbWB28eQ6omKUF6R%2Byo98AKpIooBq%2BAWuO6%2Bi%2BZxs6mxfd%2B4nQsJGhF3QoT&X-Amz-Signature=612710c609d9e5387bb018505f2ea2b1ea2fda9e4fe8bfe8d5584d615849da56&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

