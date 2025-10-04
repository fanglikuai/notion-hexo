---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6KHIN7P%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqugIpWJwb8OmuHLXLDXf9CwqX3GOmejYJP%2BUk1k2FJAiBkIMyDuYbJ1QxiV6LFKqA30Am7hDAlYCYLu2LrcXJ5hyr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMtOmmmcdqbDCw%2BQrEKtwDL12RhnKlL0Zy%2B7urnktujJyidVei6W6VXZ7IVHq1klalseCQs2RGyI3qHnBxBqtJBHVtAoDUfVQWZq2GEB40O0z5frN%2Bsl9gEADVzXCjHsv4f8HSbWfBVoLZnOgBA%2BtiqTM18llIO5dCL7fUEMXTTStZMIbVuEaLZfx1UGcpwmIrmBSjVW91ztIu0VfL4A9gC4F3bZChewWcQfk8WLZp9uNmYvpvEOgH%2FGyutScYYsoh8k9KGC6El0LOxUV5OXLn1WGmasOOQI0JjQjm6CEmFBWa0OF4bRasdbixDclI6tnmgCcB611romnl1pg423N4bRxcnFhfKJ%2FLFzd7y8nQ54S%2FKY9%2FZzabAH%2FhEAwJH0jW4pPXftElFwhwLhmywetFcIHwxMgyY0565344ugs1ZFfby7zSAs09FOvjtmN5yDjUi9e%2BcHiilGFmHraiy24YNvTQcSp4H%2F7%2BRp91hJkR48%2BPbAkNqQMfdpRE4Pxp9m7DsPqdDiB9h%2FlTNM28wE09tIPh%2B1O1hV%2BNYdinELT%2B3ib%2FU1PCSphIZsZLvuejqPhjPAMJR%2FoeNfs54%2BAkRZDknUUHU1jH3wS%2Fl5BX6ZGZ6zjUwS%2B4G8mA%2B2RTC9nuIIlEKLYjybneKHn5tIowgpCFxwY6pgFMoCOgOXBUclaLg%2FqIzhRtxUtYUoh7npIUBgDiI%2BrQBULSnmZSObR%2B5w8ud0FKBx0Omzf3iPNxtDR3rF%2FDhtIGQHxdEB8Awhag8%2BjXxDNP3IdmWpCI8TDys19mBKla38%2FYEwKfhaGneJxaR3jAOKxvByfcbWKtQNlWiK%2B1p96c5xh2iQCyz0n9jar%2FnEkF%2FznOfrWLSC4n0ugx6yfgiS0IWGSv2iEw&X-Amz-Signature=a588725c2deff8c8ed378dbf71e7ea8a67eb1c7220f545e53255efa8c417dc57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

