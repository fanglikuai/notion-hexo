---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DWQSDDC%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCqaNWzcQtoANzNzd3vizcJ%2B4URyXkybkYKQ1EFiJg1cwIgMMsIgk0i5M1%2BTu1fFI4NHmkr2c4MQaM4qa5YDCdEUT4q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDHhuLC%2FcjUko1oY8mSrcA2p88b7JO87dJQjdKs3i0dbXGuX8HMsxmmzXM47bJUKu0OJgn2QnkO3qXtyYuhefs32ZCOaoT4s8avi95PJEhhhbvncfND2SalLeimjFUuJPbDfAQCrMdI8KWVzUv7T2Eh444UC%2BRHvo%2BKg8rasLlX9n%2BEe36j%2BqPPlk%2FEiHCiqRBSVLMz5njy2MqvVjEGLvQrtwseSXyvdrNzGcJQP3xNv10fsXN%2FrD1nKZ1mjxspEVAwnmDu1%2BE74eiFvopH6mC0qeOcWy6lol8QZIxw6nO4LHsxe6oRRtfffIEpManhFIyNgQ6E%2BBOhC3Tn45vAE5QmymBRhiO%2FACeaJOcCy0ujwvukjK4Zq9ZV7blsO5M9uzKkzcXO5pKYKcJ38XEmuXZNZ6LRWuM%2BLvJ7teYidYeuI%2FP4OJOymJ4bRtx0g%2BBYUn4Gn2qTpuLMYp0TpSPXPl5g%2BywFMd%2FfhhCfijd1F9Gbi0N31od3%2FumAVrcQUOvJy4OFR2E%2FwVumP%2BY320l21cu3kQSBuYwNOlP0g0GmZHuqVr78A5P78GUUScaWcSk4j%2F9mt1uZqNoc%2BngN59mo%2FInX8feNPtAZVVOOQLbD2Ueh2MkG90ALIpP4f0dIg2F6gllMxkNI4MrMj22cVWMK2kv8YGOqUByA5J14bA6jHqjRCAUQa7oscgYHiMP%2FQmRa%2FzZe4PP7NAxumOJqSMaEM9GScFL0Fj2ffKWFyqMmU%2BWstNQVaC0YXfxIx5htfGN6zkiXvNrfNIXBcSsnx7ilpCy8wakSdONx8%2FwfQFup8jrXz3zcmNQ6E7Cx1uL7wOOl2jvFNjf%2FYbGS01hgaowJnbCIUPgJQ36Wda3%2BqRzAXKYpjjWtj%2BURKa1hup&X-Amz-Signature=add57f646dec3ab8dff11362169722711a6b69b66d4c3cafe24fd36abb25a733&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

