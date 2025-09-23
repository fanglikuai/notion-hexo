---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQTZTIQ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDk2aTBSTAmECutTU4Cw%2BhJ5ITOoKILCgrZovIK6lC7SAiEAtg6tWSly8HfKCTPqqs%2BJag6K7tQx0f8joteUQcBGbhkq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDJsXHjZ%2B2Zyq6u20IyrcAy3VV8RMR0J7f2gRZjAwHaKjt%2BwNu7IZRwtEUZt3Nwqi5ElFLf48kA5aMIr1UV79oynh053lhryFOMHPrAjkSTkAX4hOm0bPJ6wOQJjRM0aE6BmdNn4L6qNSIz%2BMdlkjwuqJ3iq2jnThXzW1DBptvJGLuXZsTXhPqUBD9sh1nllrcJ30D9YOJ6aZZCSxWJL0uU6L7ifooLERxyrlR7wor44dyg1DOqSbXzFnmnCgDbuAPtHabhhB%2Bw7wndhXB7bTXco8NHlYJ1x8maRXgOn4rE4o6kzFH9Q94URbeb%2F%2BBSljlHhirfQ9%2FiVe4jBtS9PLs5%2F3gJBFYJwYJ%2Bdken1nJ%2BcPDkPD%2F7JilMoL1YzWKOsvKRCejUD1NV70wbziuuEZaBUaC%2FFJ2XgSVS7P9OHgE0U2FF6OJw3QAh%2Baxj5lWvskr38kG33yP3z4shBXZE7eI9x9lb6neJAHD6%2F9L8I6rG6uqtxEGeTLxsTB60E1y1VCyhpUuzMjEySj0tWuSiHQQn%2FJ6CgvWjQGDIqfS9E4bVqtjyeK72CAR8yTLvy16D4vC%2FzrxlXrZm8D6KewOsuVK%2BylhfiwmxwlJTL9hKyPOneu5Nw8iyvNlGJuqEtqQIz9cirNDRvdKTmLIaJZMOCtx8YGOqUBIz5fEcnlSzBUR8GeHVeeQlqL0vKadIWJZMnTkuY2M5%2FGxTHPn1TFttqrqZT0%2BqoJ2zZFRTr1ZTYgruBvwSOPrRtGqykbwRIMTrctU5pXEDMeCqxmLZrt6SxnulqzKPjUcRQjSCaM8OUqrdg5TE0SVRXShj0DO9%2F%2B476jx5bzXgxA0ynPJV0wObAgrylg89NgIHt%2BLkHicvDTLba7A0pmWPXIQbgu&X-Amz-Signature=907620a5ed3f7488e9b1d20e951713318756a142a2e0415dd772344cd0d0b664&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

