---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6AYBSPN%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUBk07B6BDX%2BFhzOs8N2atZ%2FYNa5u4RygsElTahL%2Fw%2BAiBHDIYEmhuQHr%2Fci2oAQLyzXCoar3pfY2XzHTxZ5btqWyr%2FAwh4EAAaDDYzNzQyMzE4MzgwNSIM7wxbpCi%2F6ZXMXWqJKtwDuqhhRvdlfGRSTJ6TS3B0hQqt67IAjDEQ1X0R8RTtr7oFr7%2FBSWWVa%2BiEgVKkYCRz4mEqLADtNZVgK7F9rMOYvRm2fUvwPwTAajyBwwUQYEPWzXWeK7zGw83HdmA18Jn7Wa0Pv08tsbXDnHHiDo49BSgeMDtBJOzHPpSdO104WLE9voMnwrYihSpzpQxjegiop6bmEgRmMOlICUvXYLHAPl9x7JkzxKModOoxIo3PYReDSHVWNusH%2FWHZlOFJcmisZLNCpWFj1vj%2BMIW4L%2BKzklOTXbTdG0LC%2FR9QanBkzbBBXaxZ2tsZvwU6CTtJDBbOgh0Ve5H1NOB%2FxynmY9c9IeFRS2DgUNiahKCS9TZCfax25FT0ZkKkJ8WolD0KXylg9q9r4TiCYKaRWDDV5MdOjemFHvuOnsrtntqyXD24cmJ%2FOhi5Onpghnv7AND%2BDtMHOVRmhsSkhBdegUKC0mbdkwIUs3%2BGaYD5pM8dQj37QJWXkfdQqH63%2FZPB16eNTiGG3LX7F3Ggdy8283lXkhRSYUD6%2FB3n8kZ07HrGxNU5M%2BhbLxECe5kKpKJtpXgliT8VskAHzLcAbz%2F1sJbv8WQhm3Z9x5iuC2SY3ToQwTe7WX0WVZLma0rREk5F6kcwsL7gyAY6pgFbDX537sJrr881YWnGSosLz%2BOzx9kzu%2Flt5iLXC3ee73vrqMHDky3TXBHsjNmff79h5vKVbcjO8OqnWoY0WlevpV5HU9F%2F4hKikx3gGafwMF0aO81KJAh3vi7DDBCDt74Qqb0jzg%2B6iPyRVN8xwLx4JQ4Mb58a4Lo5ZhX4lCg8JO%2BOCYOS%2FtnF8RQ3y%2F8%2FdOO7dD3O%2FEsL1rrgyKK794ddTdFDVTFC&X-Amz-Signature=3d6c328fb5c708aba2e881bf3fcf18a7ab7883f77e4288ddb20fd317f85142ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

