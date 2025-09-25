---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GUDNUOG%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T100043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjQiEnXiT9qxqmO3yxK0c%2FSZraOEZrfmsm%2FheGgKxbTwIhAMhsOHU8EQBQnyY8stO9UGo7evf70Bm5zaAcQ6L6kuOaKv8DCHMQABoMNjM3NDIzMTgzODA1IgwcWHBL2E0aqqiSFBUq3ANk9a%2FXE6w0U9ZjwqBPWdqvdnk7X4tV5ACuHEPeHs5nWKaTSRsQBTnUAUHxKJr34nC6w9mnk21iFGnnG4M6MkEsW5SKaEtXqcR2c0FzIg25GMP8K5zYIEmpepLZKhVRjFV0tuUrR3elvC558ul53Py5n7fUrRfrDMYKwgA9TRag4ZqDwqrLXbyilR%2FTm42kklQhrZxJPYjK6H3r8XptpbVNLrSbbXYJH9CNY0d9fLLRnNhtPw5GSKfh8z6zG%2B53ZjqjginuH8Jbr4N9nLVXTWH3XhSuWrw9PMs%2BCHvE4a3AMmC0xQ2RlFktqJcUnH4Z7wUvFpL6k1dqphTHgXkncckgEL6R%2B%2BKqYYIBDUGZRMIF2HiLwQFB6%2BqVfHkJtdF1aVmRLUEekf8ib10jqk2AsldsOW93TeLA%2ByaY17PiM%2BUTnH5OhnRXUDqrCXGQMzaV7m04TJOl0w6ZRc2Q4VG3BjN4Z5Qj02LXKR6xz6oDLSENmm7u%2Bjjcutf9NieEG1EiXMgzlQIBmjSI84FdNqrekrHsMqHiQmHQNbHYaRj1WpMe2krLKu49CwU1B3DXp%2By5nUe8nq2cMhXoi1Vr9pf%2Fhtl0UC4EZdK3bKyV4wzpHkT%2BjZu%2BEw6pA7fDLNyZvDCZoNTGBjqkAU0GKDFpf1Fhp7M4DYTeCKuI%2BT7g%2F36CSqeDHqBJPgMG10cQ5enKelcKuYqoQ%2Fi4iaiFbjADSr962Txh6M0IDv1ZH7pXXuWHOCpb8dhPmYbkq4o%2BPXpd7UWcR1a5pkbRMMq%2FDCGu%2BtJ6utZtmNajpoOt9Mg%2FNjoTadrrFWgh3j5s76XAvFMRDtrFwZ4JGUk2Hu2xd8Kb6bX5RafI7aKTQDuAZIPO&X-Amz-Signature=cb0683ad8c5b92e9e93c9b0f1c74364199c83fd7f25580c570f49cb9397cd2e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

