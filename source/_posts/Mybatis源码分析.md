---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VICT5K44%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCIQDkIZH%2FgPl%2BbCBdwzCZs972rZ%2FCIgP4AvvFyRnv1m%2FpCAIgInLM%2FEFoa9sihuqeKoyZoPIHbC5zkoutuxYPG07oztQqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMzJ6XqAmktIAHohRyrcA70Ai8M1vo07JbutNSzZwMnoEZzH3Ev8tY%2FmB9wfzFNz1Lo7WO1%2BVM9VQIzfleIllhhZ97DxkWde6ZgxJBd3EDHLomLLiSy3HWI%2FaPwbRTIIXQiJ1nj55YM%2BUJhJU4pOOi9%2BVWnMTGwGvWqMpsj7M%2BqLUqlSdRx4GTT7OmubHYJ4kSUevFSM48Ply%2B6NsxZtubqkDYTKmm4cQFQYde%2Fxtp6uBROiJrECTCKfbZ8nRnxGsmQMXSjtqgCBcay06mY7UO%2FSm%2BbWPWuWDOp25%2BkUH52o9ksq%2BSq7%2BYoZ2MzdA9qnA6GMUxp2YE6Atn3%2BnDzgHtu7FuyAvN5L1qgrwPdqy2wUev1H9oVF%2BMpW38fa8E5Py1bJdNKkaH6dkfmO%2Fz0cImwhtb3NZeOlt63LN0SNupopcQwlraWwvkYZhSFXzVNtF7RMalzHFxqTAPBuSfvAv0bnJxqMLxRA9pXRPswPRQHJIa5LVyW0SWFdOi3rG301nfOFMxC7nb7vcawcXiBZDb12ZWKZdhS9Gioah0%2Fkg04Ox5r2yxtTL%2BI6Y6CwWWrRnNAS06B1rRVe%2B34dSGFUz8kzCeBIUg%2BrSOf9rBATOJDtQU5bXvJZmquhTRjN73MqjGMpqdxzvKlnTtf3MOLClccGOqUB79c%2FLPUPBIkPCR7M2a%2BGhTYGTz5bVPSoZMfQyr6QqSRoRvzJCf4J53BfjylpG0xnVT8gglJMxg5%2BBlP2%2B%2BraUXKZE8HdSbz%2FWNtI%2Bs%2FcbSiZjHcStdQzb5QMlQicPuvo3IXieT1po2ODkRMcJiGuF%2Bc08u2JVWjvGlSlM1iq0Qrs1XiZ3S3v9B8qWTCNyMJtq5%2B1z4odHvUNogoKHc6TW%2B%2BbY5nV&X-Amz-Signature=9d9578616b24c42b45d6a24509581e8d599f7071c19b40a9e54a44393c2179c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

