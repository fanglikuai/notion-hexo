---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY4MIWO6%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDHAAPQzjWLWmUlwpRfWX5OVDHc5gS5XFcY%2FEHYR3xX5AIhAMYFYqqH156P31exGmcw4LR8Z46%2FmR3FIejh%2BxzBT6MZKv8DCH8QABoMNjM3NDIzMTgzODA1IgzJNkJ7RdFQojJ1XuIq3AOkKWjBesr49Nf%2B9klJhRCC0WDeJX9NA1FWEc7%2Fj70ia76iXH1wlMtw4zXR5525XPPeOpyrGqlvLqKzeTWlTQYeEsjrDQALv92Mkn0IpUwGJ37cS0hvrtTyj%2Bm15UcR9040kYESJV478MeacpHJHPyBnN5%2FdfSOVmcA5l57FOuu2bU1NraNsC0qPQhLtvjz48XsOi24lLPNDETKbtnUqDmCzhDRQZSTBlMPvdGbgwjSWUq6j7GXlEeIBJrV4CrpXSK%2B%2Bt0iD%2Be4t0VsDOuNojOggw7jnKSofLjLxOghv1p1%2BjFpD8o61pdIepZaEnvsMmEgWPsc3XiPAS2LnomhsCiP2ce8N1GxssPb7R%2B5W2vtS8ER5ptZyNFXFzVa7KeBjmVNDFCGQfurc0BJRG%2BfOuMp7QsQsJl6kHnhxljVFEA2WdLi13jIfNmhUffLMipVRSrNELYPkYJacUpkOUIbNICcy0%2Buxqw2TR0LobdvhrWbL1dCVbeuQOyE8Hpn1dkCAgQPxZCVH9TREbtWxNOGo%2FQgL3b2Ju%2Fzxv%2B%2FB%2F11bLu1%2FweotOanSnUWPmQWUFlb0S%2BBLWJxcKl4HgwAQQ2nGU22u4iD4ItLe%2FIwWR5IaqtBu8gwQJRIZmwJq5EKFzCLmfXHBjqkAY%2BO5QlpD693d7GwcbVPtO80GjTH4dON67PzBSsmBodlgX5mpUhuD3oKYkcJFMLcAkRMugLXljTNSFxhvMOJJQo7R8Fvwh8NdAiomTpMoQvZjM%2FfHsnkNFUAhLUDTH4Q%2FjEYcqOFH0RGv4ljwsga%2F%2BEN2ZPidiaLNgOWVPJ1d7hFiHCBHFcek8ruKCXw5NALVCy1xGYmphudYblwErtf0MnTArRe&X-Amz-Signature=194515e072a529a57d26a4c3523e2482709bf6f4b6af1759a97be4d6fc17b42b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

