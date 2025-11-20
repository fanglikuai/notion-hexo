---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X6WJKTXK%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJGMEQCIAgVixZ1o%2Fy1mTbpqN%2BZz9xYNOzw6l%2FiAu7Zu9%2BZUxjYAiAFclwlnu7%2BZc%2BgVKRdtqJ90evDN2kt3B2qYCDiG9cMVSqIBAjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqYvZ1sKOMpkSnCfiKtwDA9%2B4BCA7OimPE1BUnnl8rug9bWMF7i0M%2BEf7HtUXYAIi1U0Wpb%2BjAWndj%2Bn%2FHilJQIV5We9AkdCVfzzpFmBxTiOgVAxrWeFyNKWmnU0gAoLqeToINq2F9%2Fc%2BBbaBr0Ho345NlZP%2BEJxDc%2BaFgAKMRZO9R%2BskCPc3LT18uIac3znPoEMJps94eNXI9Z2aKVECnrOH8eAz%2BlfYSgiBSkPeZ79AOi60q9OnR2XgTZUJVclXjVNhQlPbQDzRrZ37B11hKgqd2YTe5Fn2i1dgWoZ%2Bzu9KTbjVjQ3kHJ7Dk5e7W9wvVBUi3a%2BoPBxyZCT%2BbeaAE4a3EeQWrW3hd%2FTO%2BFlKFNclXN92NqKL%2Bsy8ZWFuCiTBQfnOrOpT6d4sHjIdkAhEa0rGtiRVh1kFAvhTqUQDDyHMQ%2FOK7HRHzoqWXsGyijAIz66mPlJiuMohikrJGRqbXl4rndupBQMStKUkCaoRNKbS%2BzFDMbp4d4WHeLmfR6kQHCc9MEMHWxygwPWfqhl%2BiHaLVNSe1HkYiMMog%2FtWfBriy9FbIi0hwFMOomjcEovrwM%2FOFkm%2FwXniA6tDFLj9iT3NAIqTm%2FA9JcBWhvGGwIARyy9EoykaYvoZF49cC4E4vWbkYmZp1zLiUDAw99n6yAY6pgGyaRIFisf0MLUprWgNpW1P4oU4bLK2eKu5JDtqrHxwMf%2FdhJ7uWueybftDYZJbFlCQLdUvF5MG91ZUvsmPNwYUku8kSDLwXjF3jOhaWOZ2gA57UfcBUg%2BcvQtDQKEru2JyfXoExQjrHdKbOQRRLV9NB5q9FGjlp9fm3h3Guh8DmEFHRjjhJPl64yK3qMZClkcdEUTwZS0Q6RtslrVf3aNQE3tZ9EUh&X-Amz-Signature=0e8766b8ca75da267da9225480fa2c38c8369bce6b6013ca539e956ac2af2735&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

