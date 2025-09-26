---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UO66UESM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQDk7G3%2BUoufCCfDkEXvL13PCL0UvYskAAHiEO0dCiRCqgIhAIEpgj3no9m4oDVwxw8ROhyR3q9E%2FCCQWjLKDeiNsMSoKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwz%2BI1T7U%2FaRCJfWKAq3ANVRWXgw9dNKBPT1eyvVu28%2FhF4uJdPtRPCKOS0uncIkRvfXfBkJV3aEeAJpf%2FFtVU7IfC%2FM9eFbuFepFp7LWSLsqq7Pn5cJ4qi33AwivSRd%2F0TcXLJtv4WkNeEkFpSQJ5i%2FDVUdp%2BDi9BDTLA8vjb3KYM7CmT3Ocmy6SFE1LG3G4EgpmJ4mrFdXFLiI6sZcFHpw4062x3mh4UqwhZTlouAOYVQSh3bAnaM4dDBN3jt447lzhlJ1p4%2FqcGqZwc%2FxvQuoBvB7vSIIJv9Hu94JDQ%2FudHp3E1O9FejeUw2WlGacBTGAfMv%2F1hnI0K6V%2FGiEjK7N8ZN8CbNsXgxeDZkPEdCIc8cSv2K4%2BDeufVbDnv9dfSOLCi4%2FXBCreXnjLVhDBLgA86vU1kXaeamByLWGKAWH2HtjNAhYspRE8XH5GbFdev2e83JIB1Pv%2F%2BhOprJDyhRGpuKHcCHwDtf7LF%2Fi3syD6X6LblvocxEoeP0qThYj0j5Nw28%2FnJpafAGgkdinq5Won6WLQ5WyP6bT%2FyzqQMo%2FguN4c0PGMAhhaqJP3q4UFNKLHtP0wsVnWHSqNhNAE2fybHbeIcHzVFgZMFlsi9xcNqzB3pJY4C2UDmmeNZuIs0vfY%2B9Tu85kcm6szDC%2BdnGBjqkAX0jVr0C2wIPl7%2FYgELKFp6GlFr9vJyPyD5g9geNG4rSgJ6g8ql9VIJvYT2g0Dy1ElPKb9zKI9iXmWtu66Q0Gymhe3ZavQ8qCo60oKZAjCoA0Ei29IDD8lBg4v0ZfyxpTO5%2BhEsm3T3bg98HoJt94D5C24VeXgf7ezsgTZhkHi%2Fe%2FSOqKknwgL5mudK4FU4vTNusC1ST1xGW13uloBqgPatDVYkJ&X-Amz-Signature=dfbc615caa42f3dce7ef4dca5ec53214b610b8e0084bf8c8a10b9e62300e9421&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

