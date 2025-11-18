---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RPJXX3R%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAmHF4JyVoRle4jvqCEVsKdlbYxGAvjrOJ3E6fkZW9iTAiAYVX9f5G3W6f%2FnzLjlUU9qBo4L65yzLrGOn24yKdOm2iqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2FVzJbdHfUAHJE8WIKtwDOuiJd8XleoMEJU0ElIn%2FYtF4vgrA1RMwBD7OouGGcaKnshngOOXptMsKAOLW6FJmo8yUV0I7cSqCOF5ExGpD7HCDRUtyfVqseyrdqAFKKUP3hUi%2BVLFHZmpSG6JmPmYNOOfYAZQ33nKfEiuIN6Fdehx9JhzosdNJSaWAbDzY1MxCN5G8cx9VwKToEQ2wfp0JsmlIH0c5Zp8sCPBb8nqFh4S7N97Q3rXDEoATIbK9W9pBdNINq%2Fw3SjuAjzaIYT374zraP0vbZLo1An2EJHbyplP3smC3gL2JynYYE6vd5ISrfYrC0ImULMtKAEuLJquCaWCh3rCfEvF%2BfOd0PH0C4twPXO9a%2BQK8bnVPYPs7UjyHiDsNDbiy%2Bme1mAzeZME8dLWWG949gt1cIShQ3WkqbFlcv7xio1LP%2Bo%2BW%2Bsy7gbPSo4hfs96bGkY0pP6lDq6mcFkLHX3EdyJNlJFUibE6ZaZ6pamkhiwWEn0PiCCEtfeHV5nB5pXlOYgeQBGiBch8jXx6K8TL9b4sh%2BVtQSSyVnUxQfYluhmVW1%2FjjoTxc7FeFjT0aMxPf9YFuOdqh%2BfKpd9BZPmGcsSISUf7qrODE4kYFQfQc6TJE%2F7WVWgswASI%2FvC6%2Fomd9MrsBa0w6MTxyAY6pgHDYy7cHLbFRb2DB%2FreyLrDiCFi%2BLu36tZvhNFC1jS%2BLkIWl4qKlY5ZyYuOlQquNbqX5U4S0HcyoWpYoUZNfbUAiPO3o0e%2FIUa4mvFzUxqnqX1AIWIkEBi1vYxyMCE9IsGYIAGdvippUZc2NoofpbpnVNjKUbxNGNu1R33d5tYX5jof8tdxOMB78C%2FJ8Bpt%2FdZYhaP%2F6cKB0nawxXKcnEfSP%2B3s5n3B&X-Amz-Signature=0c1a006d2a13ffd8ebed347a6ee8ddbed1d188ac84dbcc8b4cd8929dccb9bb3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

