---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DNRWTDB%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH8VwN644h6ht9u8r6pEkS3QCOGA5r7WUVFDRG6TlgDVAiEAqtELuQLQVbxOKUCxQf1SaNXfk1v%2F9yKoBnnJVm67Q6UqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJD5mb%2Brk2AMMPw59CrcA1KOFnvu4y9sUi%2BUXSLCIAdJblUyLbdzRgN0JvbkGyRczTXNeT4ifEh07Wbz4I0%2Bd%2F3ebA6kghM0tTcI%2FW3Im4iNY6HY20sL1TBLnEdhuHwQtG0D%2BD9gisloh3QKJJNcVCJwzRPpUAhyv%2F03Wd6q95Zc9DjKZ5vPoaBszY%2BXY74VNfZOw9undw4xghtr5SbHJnW5UESLn1Hc3Fg0GTk34n1IU3OxkG7tnJugJyZkGaspTl%2B5Nm%2Fsig2SgdZGTw0W4PmNwhcvKUMHmpFqZO3oUskHJGZvFUmfAO09AhHP9reqK1gBFf9TikrLjCsjoW%2F8%2FRrOYd46rFWuphnzYrrepNeL12WO%2BvXUwR0JxV3LvLv6gbUQGXU1JdxeM0zKy%2BXrqqOBFVbrlLjsusDI6jMQI%2Bx8o3PSqFcrsJc6goroQp7ZnMdwHMuadbNOXOYc3rgX84C951wOHg6L3Md6J3i%2FNWmhwnwiPCCj8S%2Blav8Lzs7Dq8sZt8g3BkMxtUka8oM00%2FDtYy%2Fu8vL%2Bc8IVmRnA4QO1KqBhfUAJZIDEtq4gvXy7Canf7mzNXZfdoszdaPexjPY373vywxBxrE6iR5YyN%2BnrMEJXwedVBgfzTcCdeCAykY%2B6vszwMD8nL0MDMNPBxscGOqUBQLBxKq6q9WipbdTjOKLDXRWY8i%2By6akSd3%2B6FOUiG8%2Fe1dxPpqt49flNo%2BbV%2B%2FVADunV76JSzbR2szvRkEmC%2B7pxdZRIF6lisdeOzIWe7AnTrrRlyoIEWL8guF0bbplX8iEfA4EXauyE%2Fw8xmbwdqT9lH6BWks7cxIYSgP1fy%2Bp4TApS93NiCK%2F2P4NglzqlF%2B3pLRLgtYeOIDY7cy7J1VYe3G2V&X-Amz-Signature=de8b64104b85c170d3601d99c88d6bddfc46a10403b5b08c1807fc7669472bc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

