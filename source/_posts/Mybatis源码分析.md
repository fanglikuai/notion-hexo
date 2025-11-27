---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTQ2EGIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGYyHSA3jZczdrRxbVSrfPf3OyoNny683fhgHiyhhnhVAiEA6tcdg10WOCUbPafWHtgeB6IYSD2haVDyp406vlQVHQ4qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOt1LMSfAZih9DO4GSrcA2liLvrwPVt4G7IstTcnytH8238vwC1cyl8cmhmoX8Uusu%2F%2FDWVvbg9TCoIXZ%2Fdo5wu0vbzguIuhGq4JrxHG05fFcRKW9GrGCcUWsnPkM8EFsX4wBxDe2%2FrykMsq4NJHOQ7fxCTLitm%2FgsdWC21fefyZ4AlT9i2F8MpvPOalx4BVrgQ4E1Vqiq8B7a11VXVf1j3Bd8vKQ8jV0VtXl3aUy%2FYDXIILMfnEer%2FXzncDcCPzYVmsbNNQiyha7GSho6yMRn6rsLWsE07FwWo51Rqy%2B3lcDBLJPbGIzPSblqsTmdLeicTRJJ3wOdGoFtUkXwyOGGj8XdY%2F0Fgb14Xhbmw1KDujBqfml1NRpUih1ayDP9eOCcSIuz%2BhobL67VYNzlbwoDQgvUayKBJ9KXMgH%2Bria0owvGy5VIiRRz5xI5MLIMz0maQ%2BDpbDD2EuHJju0BTrwJiAYo5zh49eJCsNFmWXioFigARfPwBliH6YiW01Cu8StIR6rL5j2kVFczAnWutOXBrcDPoRIe9CxY4iCcre%2BcWBLMqrbSj8%2BNhAFphEyvIaX1qzrNoIk%2Boql7Vx0LBjhg7JlTnDY3CF4bDiA7sdgV8HUzgXPaWWd7qxuum27h4hyhSXjlE8Rd9XWIH9MKO4nskGOqUB88isa8XrHbvmlj1hmROapACwvT7d787Yz8pyp49IW0HZhagzjz2gxGiJwU%2F8q4U0oDnpL1AhQEBq%2BH1ftJQH9lWJjhVkUNEgc9dbtYcZYRYNrQC7TZo21FNfSkO1LqO6Nhvqa0W2cdfAcaHQiG44Uzm15VwkAr0xMVGp0LKesAC6hJHzIb6iHZUeBbGyptuqjEIOMAK2ptmMzLNT5iub0Kr8wL89&X-Amz-Signature=cb50613ea25c0eb2d5100a7efd15b16693e16b4ce0a50ab5f83c1679bf9afddd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

