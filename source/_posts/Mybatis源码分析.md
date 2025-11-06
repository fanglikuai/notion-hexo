---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663R7KSK6O%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAT5BzFmt86l2nKB9rl36KlrU727EPez29agOkCLQ65lAiA6oY2KJtFltBjnkvl1BeZT62bVMecwa4Xot7MPFp2wJSqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYSbpQi9VROah1xolKtwD%2BWDYQnmRKE4FJLBcTBbXVQkePrs7eJiKqA%2F%2B8vm8pTiNr0EA7LM9V6fdQca4UDNZwpyBhsLGlElbOGD0Qzt%2BRlIJTAKNA1XjXFuWgWli4Ysk%2FkvISiUeXKOEHWeu%2BgRSR6FWc%2F%2F80%2BtWlPRUzS%2FUX1krCRxxIThHPf78xkjhrSyaZV58xkmhnVFteH2doIUTMAhMbieuwWobAw2JzodFht6SdXjljiFGNTFsccjNvZYXDKex9dS9P5raoYolIzRtdDwgREEEbZ0V0YH7NzhLIYT3kzWyvpLERnt6LWE8IP08othBKzFSh4cf6EKoHxXqakDtl0FNjluT1g%2FGp1yOuktzGUWaY%2BmUIKjWAuMmYjt11KvT%2FnrY4dqYMwkQvpyFAQTC5bS9BUGixjS5VjeD6Sw28WtJ4usOa6ylgjV3FNckMpD%2FMc0ZCUUvTobIZF8rEenEa2wVoqZfUB4%2FKLgMGtOHNoKU66MyOdrRXgEQKnQsCGS4e8BZ3OEWuFBdjWq4I0xiUpNa%2B6JPQ6XINFOPYB8uwNR84V5d1vVMq4nc60Dg7uC1ZthqtBaexXKc2sdmwj9klCtvz0uX690Y76%2FIDhhjl1W5MsD%2BWyOnDuDC%2FYytSFu7aaePub2XDJIwlYW0yAY6pgHZOh90EvvBpOmh%2F7dU94AuduNKgJOyie5jyidgN9NLl2iwTGHNxEx4A0t5BDhU%2FMhcoYNCYu7x5yleJ3IX93mxu9%2F%2FzfyMWllS6R0tudrI8FLfKvakNVMPbf%2FRdgoecxrIlXEpO1SD4Bcxvtxzj%2FTRJqKJ8mCORf%2Ft7YLPdRh%2FY2uBRR8%2B8Gs%2B%2FqnsVS9z4ejioTroY8eJ%2FiU5bGSsylb9hieW8H76&X-Amz-Signature=44ee3ced2730516b96accc9980a5926e809ab06bb09dcc4fc86ce9fc2ed52d1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

