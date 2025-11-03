---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675OY76NH%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFALxfYn59Q4%2FiZxlKVhrUpT89WjdR98Rw5xnsCFD3QAIhAIDyf13O%2Fd60CBG%2Fnkor95dNnkrzlyWC6Xc%2FlBd7QSKDKv8DCFcQABoMNjM3NDIzMTgzODA1Igw5YSsT71wgwo%2BfP3Qq3AMG40q5e5sH5h8neQ9CHP00Bcu8zS8b5VZ6ieIogOF17FcynxWiG6GPUIAlniEbRbk1yI54T7oP%2BiY8GFoJ%2FSU7XbFOA1q9kwO5XuHz2bu5%2BxwfN2kanxzI91CCfP55g1UqclpSN0NWNX10jwgyfkOxGxZfRPDuVrVqK0dIb1bKi%2BrIm49gsw11imS8o0OdptdtfatKeduyboOUDn%2FDi66xmJ6H2XLuVhwsQkwSFaBkvh578gO3WeUbC2d78rASVow8xd8hwVMire9YpZMt%2FBzH2C4UZ6WOMfwM%2BF0Aq2IlgV3Ou7irnJBLE2C5z3e%2FyTXxAr6VrBw27QkSLsbo0aUr9GNbIt%2BnvxrNASetTUNMeJWwenHyPRLQg29IYLfAeds%2BGr%2Bt19eiGIFZDeVXC7ItwWF7DtssME0PYOtn6bOinPApkiW7k95oFU19PiSmzMkMfU%2FOzVkwPNB8%2BCYx0BrfShQRl%2BdP6GAlXN1%2FnevU0pJ6VqYbci66yi9KM9Tnw4yMGuJdyC5jwiLBfaECj7RP17Ym4lL%2FYEn1ihM78xUX79Yknj5YLLt%2BFAezixdJY3TaGyssKh62WArLrf0GG%2BpOEHNJlC8ybohQJ5ES4OQyoFrasBbNwhA%2B7e%2BQEjCYjKHIBjqkAQrulMchr%2FTsB%2BQdGez39oen5lRZYUeQRNcruJaFG6E71IP5YsxeKsz4si8pcjT66Q5Mu3tyYxengiRiYSsmvCJphVVJ3S0W02bfmzOXDnpGR4OgdzpE3qcMOGzMqQavGx0pfnbrSHlPJq5BWLe30HCXaohf4uPqnRcaT%2FyB%2F2w1M5o6FV8sXFHuu3mFIFPLWWDvcwDh9Hw6W6qa59NvOD17jDZp&X-Amz-Signature=0ed9f4e14215bb3ddb5ccb8ec8e069bc55fe68c8ab83c4cd661c384cd5ba802c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

