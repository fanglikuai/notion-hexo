---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQG67ABM%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBe9do6qoyKN7i7PervhuY47T3oMkNfFERBbUgM0euOtAiAT6PcOu1Kup%2BhYFIh3C%2Bv6xRCjoH%2FC05R71uya6taAbSqIBAiA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMA3e0On%2BsRWQaTlrpKtwDic1qMa1ri8zUU2NbqiepKtKgyojFFDFxoH0DyH6Yb0JusokdH6%2BGy%2BSqkfxsMpLvCt5OVVB7luUFhHt6WAIHCwuD3a79OvYIpXJEqC69LgiSWLzi4Y0uCtlEwMEUYGWzonZ478qP6QEqyUtlVsL%2FMnLnCeFYepx1Ap%2FNnYNgzRe6%2B2breyyfqSTXOWv%2FPX%2FX6lFU9RJeuyYtMcpaQUqTGgjVqY3y27IjurHD6AviUwohE18O4xwxwtrEg%2BIvWEbOC0P2ANgqGJwS54nV4vNkee9WHcxi5%2BOOlaemTuiCvLBdoRytTDSNHzQl0omr0dWDt8bpX4F7gwrFDmgKZMFvNJYchB3Cw5rpaxDI7NBTvNQYI3ih92YsLLFAbImnaaVCRoqVI2hQix2xfzp%2BUzPZ8S40ugqeztfm6YffoPH%2F9LHW6T%2F%2F07Ef%2FHXJzrU%2B%2FbZOlxKYkcYNT3jKPhVJ3bNSrvtcY6ttJuDILtgh0mXH4FiMnf3C4qsonRc2WiJ8b6OpPxoXsAvQ1Dup8C%2FxKM5wgD6n%2Bws2zsy7XB0bSdHT3MaGkj0fjF1kLCx3RZE6wztO4JJL4kFaKW9B8euXixd%2FbC2%2FTobUwIxrwXjUYQ3N3miwVsHnrhdoueJ1KK8w4JTXxgY6pgGZq%2FAT8TGTbNI1bzFMuRw4wOQxpyBhO7Px%2BowX%2Fsyv6X8Tfd8Z3kZlxyd26vvELKhAiTUShyNOneno7cNv6MD7V1hJ3I1YlYwnozJoIpiPm4gZMnsZfpPMan8tH768onvPNWyLUr%2BMa9Vp5ip%2BX%2B0lCDcUrldIPAfIC2ebFGWzmi6fwekZ6VdHvDaY%2Bn1%2BYvZfUsJ4bcZyiYJLZ8rGe748cmxMOwCM&X-Amz-Signature=3efa48181e2a465724b236099638b4ef51505c536986e25d0abe5382adbab994&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

