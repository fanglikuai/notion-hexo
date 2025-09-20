---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7FARLRT%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIBeDmklNDxZ8%2FFOUTo%2Bcj5%2F9cVnb460A4OqQxp64GkiDAiBVJKgDnyW%2BuWRp673DxXJSCyfNRU3t0ul%2FYREba3GODiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BsoWh%2FsHzSUuXvnlKtwD8UqnWO7rDqJHwhBXoRkpByDxiwa%2BiyxOelSqyEws%2F7vN%2BZC6rIZA9Za7hIFK5oQmDn0Pua%2FGkzKZN3wRl6y9LPoZ%2Foqk9Clh2TvjkDlTQChX%2FmD3FauyFzHz%2F3OcaITcX9gm%2BMruVi1Rc7WLg1REmcGrqqQGuojr9CWm%2BJJlJTrvHkOXzZvkd9HzslcJItYLnqtV3iLdb%2BwMoHwYQauIdB9x4QlyIBOmyqfO9QumKnS6mAqHg2Voeo6JGwmlGFm4KVOHCcts778wTWh6RlXAMVDdkI4HvOH%2BEzdC7QBvY3Q7i1sT9dbOySPkBIWYFAPWXKs%2BiXLGlko32o65XIL6uNDUbq7KlRtDeFUFQXPxy96OXE87ujVCOxZs8tNrJkCUkkO6mB8Hr6A8MyGbk0M%2FxQrkSkssG2ul4Mx4s4DTKmrkqqLqjE4pbiVt0%2Fj25R4MzPvcSSMg6KgOAiyoisoow7Qauo0xQ5vT900Zxv5vwof%2BB1Q41w21xQQuQMHsL%2F8kit%2FWFva7wQ9jNJlmfP35v9zOKlNX5%2F9mFit5yZS9CaOEV1cm1NvqajHOXF%2BlrF3MAHhy0%2BXFvzDdiL%2F5Y7sFOJe1Yf0O9LOmHcd%2Fg5TPsG5fLIODincD3TEIBKswu8u6xgY6pgHLcVlkAOyKy5YIY0Xwt4f6obIUvwG3I8JjDuuodb%2FNvR8LM7AJUK2A2sdPtPau37kLryBGocp1i2vb8kjg5mwmY9h6fBk38IFfpdbHIiJtAap4eG155iqCF%2FF%2FK00fIeT2kVMlu7aPb%2FxOcrFQ%2BQhZwm1jL8QAGn%2FY%2BKTtmKYFCtirhET5JjNp8Q9VMIxps5vc8EMFtHGYPoSn6H8hTy9hivWFmZfq&X-Amz-Signature=88dd5194fbc054a43b8aa9d3d817c0f52c6c275812a2a581a56b3e1624724669&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

