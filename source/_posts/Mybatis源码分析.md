---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDIW47HF%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T010041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDtFgSuiLMUozH%2F6ptq6WgdjvPe4chgl1KRIKkU0KFqYgIhAP0BoDDH5%2BKK4KhqybXBPH3Lv1Hztuw9%2BOEWPtL7ZZJdKogECOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDDTwe%2F%2F%2BNes0hzUq3AMNAAwMwjGxAfX63YEe7E%2Bqf%2FmbXvXNJIOP6wQbbh94HcF4ukgLTsJ942peP9CaveeV28VQiM6HiU0z32VblJJuZ%2Fv8qCtv0U7v8BDZEKWm%2F16sIROJ1woYUzy96mNHAYICoKXqFPdU%2F3gRlTtijDxIn7bSj1j%2B6P1KS2KCkxpPo6A2NUCofDBV7oPhk4%2F%2Bo2EwsSaWm7vlHjIc2Wo6G8b1CzbuqVYqAc1WUUpnEp%2FeCqo%2B7f%2Buouw4i5hWmdefBHUSRRHDgYGc9BwNAdzeVMyWRG8dr5a7AnIimSzZYl%2FEYbGPIbwA9nFDt4qMT6GEDldPicsazAj4fg%2FqW7CC%2Flid%2BwOAtNZJvPx9tJwvTUhhXI1Gfw%2BTztoZPY7SRRigygIKvHhNrBCyc1vdchHFk0tEFkCX5nxDFr4%2BvzcAe6Yh8ez6K8sVrAm7VoNwkfGY%2BppOxGgIALGBuAdFQzL79tXqWQcsY98aDISP1HQSfXXJ8rcIKEqdg9H82CoL7pNHEN1h5crZ7WFBQ8swUHaLODjWD42D0y580PkTsEaCVdl%2BHHek5bbQPu11vGBdxOvnXVD1SkEHtxc%2B%2Ba2SL1Oj7F0E7NV9wJJAkDB3xld62C2S0AeeUe%2BIRZ%2BxtthaojC27LfGBjqkAQVvPfHensH9%2FQiddxNdqdtKYNR%2BE7iG8D9JeYDEfNqUsL0FC3FDSwA3KWvbEnHu0t5W31W1yyAckLVH6dwEp%2BzZqcjhD6khYyZIwAkq96VuD1huKZ50YDELlOU6HCdbr%2BQQuCmzeUrUSVl5K5OZ9ABt3BRL3BjGwiyXQaPbULxeJx6%2Boj3YNJt5c82%2FAZmaEL%2FxTKRHMnXFkH9MAb80w0CIihEQ&X-Amz-Signature=57589eb1300d52329d93c6f3c8463672e4d46b90ef90388d850381be5dc96530&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

