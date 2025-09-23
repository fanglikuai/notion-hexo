---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTYF7MVM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFw7EK52jmctIC0DXq3VOigSMcce3Qd3XlLguRZ51e3NAiBku8ziz7rBFDpwSH7Sr%2B8QChwswKpszbUqlN793sVKvCr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIMK8fN4%2BAozVzCMNvtKtwDFVg0fFMV%2FMNYRdnvdU5M6A6SBRQZ9cPoJIDaF%2BxUdiQtyOMqf70Z2WA1p77CLUhCau5vnO9PLdUsK5%2BIfYbsS2YTUTFV%2FqzBqN8SQeRax1uYr9AsF5AgD%2BIf9RcN%2F6saN7vfZPPtoTayY5nUzbaF7Qn8mb2skzGAs8QEJh0%2FNtLXaY4teSiRY3qPWHujrIZCPFvnmdlD2aVO3sJnD6GGCnsPSrqp0GZXJ2k%2FtIePC%2F1mnj01zUJPNVRINelrUq3UikiPMS2IY7Lh3AjM3lo3e5pDxozgwmVcQJ43wsPkesDU6pQ7fOx2XP32L0QJpwWFPT%2FPsblvOG6%2FWVRPG7QuQYEfOoxkjtg46lpG0ehBqjRQswbMRmD8e5YudCOHhFI2Syp%2FeVYTYE%2FpDEg8G9g2lZPnYbKR3b62Y%2BoHPjEk7HRZPyH%2F4GOd0NCVpOv7vJmqibdAZ9h8ENitfs85yVtrGMqwR4lMnpZqYLXlZ5Mvp20nQw49KB8Z2vDl%2B4oOFvRaADZ51XZ3orGMnSM7WwpVsTmurhaMPhWHc1uE702cgxlsUi9hwivMVb39NmgrLne86aznlz%2F238EUNjkVQH8NcxHxBfXK4xJysc8aoDB%2Ff7x5ynYO77gphnnqKxkw2vzKxgY6pgGtQcbhCXmATf2Ccm1kBn3ufq4WxeaQNmLX71Z11hk1hLE1VjB67U6GgI68WKzQ1UrJRFJI%2BIpeVxxwcrFSAGRHra7JU8yEEE%2BlfkiQ3GrRNVWRmXKwfKYLnE%2FTVsvWrjsEELVVrySKZ04QbD6VLpZC6uWgnoQk35CnXPaTbxItwsWsmlnt42oRrPnI6wa1uyJu3tD6DzPsH%2Bhzp0bt%2BSHS33AQ7DX6&X-Amz-Signature=e7647bc743c9127f23cdad4abc2c3a6e587ab1eb09f6e387f8cb51b94a865a1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

