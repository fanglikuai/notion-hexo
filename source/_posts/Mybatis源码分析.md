---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSO6Y4NM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCFLsDMkxZ%2Fp%2B0qi%2FtmIr%2F5jO3bgSc5H%2Bs6lzKLnpUB6gIhALovxr4yupw2b%2BI%2Fk03QNi8XMDklIbiii8MUS4mHUrC1KogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyko7PcjKHoOBh7l1Uq3ANoyk1iRlnrmVb5vyGfC0XvRidzTdT1hawJfC79TjxsImEDbDcNIUG0vr%2FucRq6lQvMu1TF4aRS2VElqCxrZ2yh2neKWHx81C6P%2B0eysoykCof6yJO8Ia23ph3BtXL2eVZLOA6VtLWCr4CYuGbr49tx77QyCcqsHdAfJ72DEggVHUhFTVvvPFR0jRCKiuE3ACLDbOrmNq9tl2NMEsdcF2%2FJ%2Fh3pSAEHinNUCu82RE1V4bwju%2F7TE0wyM9VIGgUdyBi3Y95TNejxySzM5JWEaw8STbd5ytuGCXGVi7ubREq1dQ2PJZ8YWL%2FM2tDFxXfdW4uu3VvEGa8oTZOAIeeddaEgbEPqJd0%2Bjz5nYyaRCIH3vhUx3yPxmhzFW%2BkgsKnXoqAJ8AwNB9eJmY1qxV6ZFPCiw%2BAX3eeampUxZQDrqnqhr%2BKGcRZN7FNmecBb3T3e7bljchqW4gTJJ2oI9szv5khz%2Br31s0ku7Aroc4N8VAyt6DSkYpeD7fsdbxcFkliw%2BPQeSvPHUN69GsUPgw7kJecC8pcBKRjbZKJ7lun0O5sgPxUqPjl2Fm1KfNhynNFL%2FuySyNGzl6pGe79wVK9fNdVuY99l%2BUgxB95gmOLBB6K5j6CgKtGMGy0x%2B%2BkfQDDQ%2F%2FbHBjqkATghST%2FxJlKTLdc0%2FzBAnz5%2FT6DaJAMHq09%2FBxIinQAeK9xsDXhWZnTccKzRriG5heSwBStuBciWoEqbmtUiX%2B8gqOs%2BYi1grfcQ0LMW5VUEW74tRXR8VjplsQm2mEZt47qgLxcRT4a8D80sNteDyfepgIQve43w6Q6MUl5g3Jtf8JcRu0eO5E4eOeiSpefmwbWy9UomGKtpEbqcfoFQg8%2FCY36b&X-Amz-Signature=27ac9748c1261865c709c6d6db9cf70759a5de5aa1e3f97bc0e25ea9d8387515&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

