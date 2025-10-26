---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S354QMPC%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T060059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDpPpGKEb4hcT1LMn%2Bz01rXuFcK7b1ib8aDVSJP1VrlFQIhAMpakNBm1VGiqBgH%2Fu2Z8bMwwf8MUjSxgdo%2FtJ2rseluKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyfl89CLXD8rvSAHQgq3AMBG76FF6Qu%2BJ09E9xK%2Bzv1dh2XDjTwnM55PBe3yLFiTiodQe2ekdrHL2cHSR196oHb4R3bqNeUN5zgdUe1LmD7%2BKw8j5wf31JU4X5CzzeDIeLUpsig9szH3EVgOWzZGhNkG2yd6%2FwQWcbjNMYGGTIRs9WBawq4fzqg96H8KxWBgY7LKK%2Fq4rY49o1kERMwNjaNIZCqaaejei%2FLaPf49OrDc8kyivGaiGfBYp%2B1KCph95o38eN3QtSK9FN9KaHblPOazPlOehMYgV96%2BNjHzxRp6MweQHeAGEle8KfJeqdCQUabLNLwuTpewagRwZaIBMy%2BjK%2BA9vp%2BBre7nkSRrORJLlZn2y6%2B1a7Qt18F2tWppqp5zboF5Vkooz0v2EgxY%2FabmdJS9RdCqpx8oubup1HhXooQvQflVAW1tP8vSVWPangLAPiN4rhasqyL4XmQvokPpEpTZTtCbUMdT6vC2P9cjM2xZ4AivvrSmY1nJRBooOOunwrWi3pxV0Grk4hIhO17GLRXtGtx3mE7%2Fk%2FctgeNirK0sODtDQxaDkz%2Fee9S3dpntPH37ViRw3IQo15b1fME%2BCzdvylMmyLiBWuA22XVgueHlD7PO2kxaVjfScbwHDCx1THGsO7tvfDjeDDX7%2FXHBjqkAYKbzm5c0g3BHkFz%2BzqagGFvn5VuJUcgqLMgeGSJbvkv2rnQNgKZXzRORlox%2F7GKxw2dSdjBTtP1WrDerMilUk2Zw3akYHj6p7AmCxdaLFWgUml2b6YeYX2w9PAyqWsQhZx%2BZJarqpwp5lnYulB6zPCsy%2Fk6lDHx9AFimR7S8MPY4o3tVoVz2EtOdiyemNuwg0gsLWLLckdUFT7gXX2yFNQOwPDA&X-Amz-Signature=b7f54bd60efdae504eefaeee0bdddca26340a62e4d42bc46c111eaf5760aaa6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

