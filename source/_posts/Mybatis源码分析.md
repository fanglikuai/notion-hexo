---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZFMJ3JY%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T090039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIAD%2F9fbrX5xvXL6t%2FnNi9dOitg0%2B8Bvvru2uW9wj9cqqAiBe6hZnCyWPbvmuQP4lmAXPE6lZVCcHX1yStwn3alNcfCqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2a5FqZg%2BouAVgNxyKtwD8xBMn75Z4Cw5Eoo94k%2FKg3US82NCSQHYN6RMdcOHO2v3rhzBVpvXiVyv9hBMXw8bIrSzrdjnPk5%2BAsc4CaITOLXs25Pj7mrfpWuxMkUa8UdGclnLIlgnAPCoTVOIPq8TUCLphy%2Ft%2BSB8pEL9Kl60zXPB4LhOIGD92RbLiUAWep0G7nboJh3S2fTTyX11%2FNIfaHm4ZUkugjFd1zK83O%2F%2FuUnTzdAQqlF2GwrJFp9rIRsALwjurvI9a1J4fy8x8qC31tfCfjM2kdhhCvglWa5c4U8l9q27l20qssGctyGhniHTe5VUg1lzBH9JXpYyMo%2FvmAKFVjiuizPoAfLl13xY%2F3XaYDnEk4Dlb1%2BnCjAaAcCtmbfJhA9RDdTJeVNQAv8LfPjL3KghmocK6CSdkIBsf3GCH%2FmC%2FdNZjk6ShebhvE%2F%2BPfCTFEdtGqrJnZ0OVK%2BmrrPKQjy87vwaxCSoZXGyOml98eIVuCzCZclTXvU1Kvn9Ih3BganYhTeD8xRU%2F4%2F8eaxbVvuqkQaJeleeZJoHvSJirtdfj0AXt5RI93gXUDvK4Q9C7SeikhYK2SiN%2FVkEtuKCSVu3yTvHVy3yCi9ygEjLWNQmy7eTkwcZwWwQZCWgrmleDBqlddF2274whpHpxgY6pgGQPl7Cju%2FM0trpdw5ahaYKl8DJaIEsh6xouKAEbEgBbGOAiruGAD32n9fgWaQB1KceUrdOk%2BSB8%2FPuXOZzbk%2FA15w5BMg5SLjPDlK0i90zF0todYPyJHKL5nxmYfDMEvppuy5dU3ud0nqQR5r7Q%2BGYJCLi2UGPkYyEZWJA0P6jONVo2NJWmtQRUOGijF7bKMseIkiPHfIOrJuc3X2F36HonUah8In7&X-Amz-Signature=7411c5cfe75e52c4176a5a2837abee09a206dfab08fb4e7730c8428601a57570&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

