---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EYMOJJW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIBABmpghBO8x2abjNlrfL5XauG7w6TENFmedTbQ6ZemrAiAPwAO3eh99qe%2BtcHKPZ7M7qNN%2FfXPReq%2FxKQ7M9uLQ8SqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWuwfMrzqFGg8SgaNKtwDAAm68rJnU1uUjHm1vBF6KOJ8nmudcq9gbWyb3VakjjqhHj5jnfPqeYz1PNOkhQr7MDBJvtpH1JGgoZiOXp2IlF%2BLmOm7WOKG1u7iB1%2FyREK3wu9SVZ6imw2QOMRk%2Bxh6ehUrbvD1C7JjVl6BrJhOHBMQnRQMVAJmfl%2BqfBTJkz1qb3iWARRV8SIzVH5GdHZerCr47PH5e0ao2OnoLk2SLoPmidATCqeywOZd79fIjALgXpUFmSN7lBYULQj20wXsmfa%2BDVWvcBWypBNgDCJ12elbUD839wQeoC3iDBSmvvDp57XzZvK0M7M%2F5tTnG%2FUx81C0Q7xJHW76pfjO67aVqBGxBf6i9XTPy%2Bme3UVohFYMaamFkR%2BIbOv%2Fb7QELVRgfhAZUDMegT77YlWJHIr%2FQFRnKN%2BZUGQGi1rYGE%2Fns0a10Z6fDX%2Fo22O2mdmWyCJvt7afc8jGDcIfYldhCWITfCdpw0xEUHQRrAyBxehb91TxMOZIyuIOZiwqXNPMN%2BhZwikp10rKEDxrCi4OiZiDRMcHGLULRToa%2B6Ju%2FGh%2F%2FKXnaKCpL0bJlg9WfmLNxA3hwPne%2FEjKXGncNZMUbKjdp9etntsUMUm6JmUhhB2Qz%2FtSzyoNe5fxdsbKLUQwp6icxwY6pgHCK0EIihSj2pbd3umq%2Bt%2BcyiFBdiY17urQPIrByXavZqIRemcX5%2BvvrTPoebsPqvwC3v%2Bw7kateaEO5OSWa%2BWKzx%2BT5JRK8gGv90T0v3mEZ5Xu5aSYEU1GGm1tYBaUeFZLOtnZDVC7gK8zO%2FV2CiueOPfjLpOpv%2BO%2BArSUvg6UTInVz01109X2pWy8wDHjSG0NNNvmktrC4Q4IUGUBPEPepfbTgx1Y&X-Amz-Signature=cb9f39efdc09f1222a8af46104b65149d00ee89ba751da975e39b51f36b2e55e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

