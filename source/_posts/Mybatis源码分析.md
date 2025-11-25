---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IW4DX2U%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRHBoPktg04TKMOXxhmoxpE3vJhZAyv7BQcCb1%2FTnKswIgBxXBgxWio%2FEUbGgtcVC8JAEE3UxfL8CMFNM%2B0dytbZcq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDPnVOl09xV629S7OvCrcA%2F7Zbjj7T29nrIoMbrrxPR7LlH%2B6RrHwJ9fj5O08MT04HLfbbx%2BNokrwZYBD1iCWQpSFeeHHS7jb3sd7ijILPv3u9SJ3gLrg45Hk%2BN8xF7vIgp0kqWcH%2B39L1li6amOkR3azW3Dg2oWlL%2FRQ5koXamy0%2FKBYyIBdq%2FDy3ZXCcKFvpanBEuU6sfHaWrp1Nq6H0uqDDd%2BIHa24qiOIvvwrNzRbh%2Fiq4O%2Bb8Hh7WCoJSXzqNDHa4T9l2%2B8BC7BQEeIweWzG5yphywzbPZquYFg1shQ1l%2FJrHC3z7gXpnO5c5lAWpK73vAoF7xewU07cR2wWppDrdxtPuy3QEuYyI3pKUtD18Gf8FlDm2GTcl1a6IKV7O8mmcx6NSHQCc1aR%2B1XaZrXasqbZdZJlhOmAxTiUTSWhcysbZ%2FL7H3Hsf7T3Vy4l5vOOfT82fojZWe8Zl1cPB4d9qBoiAaLnjmkbCsh%2BkjiPAHmMIb3z8zaBQHVbwvZc9XWd0p9%2FYZpW6wnsZ3EEM9AOEVL3I69yj%2B7phoJnmLsywBGKmtqgNILg%2BLVAdu4krB1oZ2UaeuCZGWWKBYu1KWMv%2F4U9uv99s6XMuqgMrHBN%2FuCoe3OSzNhlXcNC9AoUJ89Ces90NJ7XCpNsMM2elckGOqUBTqhZdEFGPQpK8s0IBGwrG9EJElZUg9R2wTjCoylScNRtXro%2BjNzvXyOKRFWSkUuVT%2ByBpzECp58070eKjs8L8Cp9qWUKdUOCIkQJlqjCrRSTjsEIpQysAzTNyFAZn0wwWgUcAs85eTWgWEYS55RL2pAx8zkhY5wGmWfxbQj%2FgIXQHDpbLxWtDUNlpWJqwCl6WUftOD7hqii9LTcZgNa6zTTz4pq7&X-Amz-Signature=6670ad069268d2000d7ea65fbaef57f06fc700a30370f911e9b0a61d25a1d43e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

