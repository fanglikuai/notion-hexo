---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YHHYKYR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEWQjeQ0nRndFkXofnupw0PVDKBY4n1qH1Kut2ga9NwAiEAqoVtMVc3BwSVAdRlP2rlyBmo0bWAaA%2FETIMKBIqKk9Aq%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDIhuHWyTxznX5LQwiircA0PXztK8xQdtxzVsn4KIGAqDTdDXnpC86Pn5rRp4P26%2BYIgMIGi4wbzvWKrWrPGXgrbjyib0Wsw6VTCOaLcyHVoSGXEXyzSBMSywqT1Dvkq%2B9la924zAfWSSx8t0hQzrnW6pAidGPHfvIih6b3w3BDhogYBAiLxzxMy4pcdgMftJ%2F%2Bf08EGp4oXCSJgpeUBxlafmXT5%2FWipVT69YPCRILoF8WiH5kwrsW3ZLg4fJyr%2FnMhf806YmOi%2FTIRKbgk5Zva4VXiA7rc5VvwYD69eveDslGpOr4KODrPx%2Bx2VeHfc7YLE7p%2BCn3Cm7QLkccqzybbbRfpgUeORVHtCammeQl036KnZIIJsb9kVDebx1Ybzw5O9SxcjJxA1rUEaogBuW8J2U1LyvR327blQQO7lk%2F%2Fwu7ybVUFsWXdU13x4wCjzRPPeUNDq3htvk%2FgaL3vrG%2Fat9bsHy%2FA7YdzuimaMkFk15ZdQIFXydi14wxlnW2NOcsEsU0OQrJpcg%2Bnwl9h7iPOvpDgBRkIsZMsckbjNg1vSN1th63cc9A38FizbokvE1eOnnTlC64bemMCjIiJ0nmKEOixbzm%2B%2BhN2qvoXW1hE8Vo1WTaQyI4UxHHdQ6EHUPTHyMKqtBWsjYUsX%2FMIqp1sYGOqUBj1RR%2Fj%2FM4r7QofhsgTG64lCOh7f6xa9%2Fvwsg8QdNUVjLOTSOxheFWvr9ayqBBvUvhoUjqsw0VLDgH%2F9UO%2F%2BEXwlVK%2FlfqnImnZ9RzZWu6zRx%2Bs93i5e9TeYddvcbNLo0Yg0l3x00RAd1DDef6EyGKNEXpw7ZjbV928C2kza0TS3BKWc8XMAyL8G1V%2F0VaX4FjQO2lkA2Hq%2FZSDyWmAlmCA2kZLNQ&X-Amz-Signature=ecb89842ed69b6ef75744c6b091133774887fa7a60b02978de62a7c1e48fd41a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

