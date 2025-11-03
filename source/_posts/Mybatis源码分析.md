---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VPM6LT76%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDT%2BJktxrsBmwsF1kc5ysbNO5S0wtDTg6bWUHZDYu1vlAiEA0vyWA9jrkEATmA9dt1oiH74PdQGu8Pfofyk%2BWghe1GEq%2FwMIWhAAGgw2Mzc0MjMxODM4MDUiDIarGWX1WBvkr2QCJircA7dpfIE9UDDCpYbvbVOyeeEB8%2BKr3LLDjUWkbvfS2kP6drS%2BDYPMgz7IIQmx1rrqVFW7LXdPniPvy8XipkbMdNwiIBSB4cqLhBNsy%2FqvNDyLEhvFCvUnLZMHwXYVtwgTOU8maUajg1zWkq7zMUbgBS7mDpijYNkpJv8VD3fAE4kVkNWPWiZOnxvyMMywBGkmD3F7yC7S2xYOf3NIYW7IlR6xpu1Ad1bfWP3A0tAE2qTqizM2O84%2FP1oWAiPbQgW0uZpLz19HClG47nXsKAtmTXILCOM52bJ2mAoE%2FE5mLY37k8PD%2BlpPOWC%2BEnLnG9x2VzOKysjNcz078mXGZw1naXqu0rOyrODF6OFeYP3ACAjdcb%2BKMx54NGNtw1wKzkOaGQ0IrthzPyseHmIf8%2FLFqQoMaYv9jDlhC6P0w1J701rLkQFt6Sq4a7NCr%2FI3ZlMR8dEpXUuf%2FQ%2BuhhqjElGQPwVuwZx6LKoz5xQRmti8eLBs3RRzeFuyJj5V%2BxetuYznopU%2FQwiDJ3Gac2ihZ0Eg5yMdi80Onlu0Sw3vzUEcVRuFc0ywT9dlgPTDtaTOzoqmaHAQhn8ZSz83ENricBM%2BbZ%2BqBL13QxG5dRbzZcwUpa439%2FG3jP9D5TrODsENMODQocgGOqUBqfFZ%2FAaGBN46fvnT8xT9zpsxOehGE9qvJEKcWgz06SHMg1BkNWvGGj8JNT8g0t0GhWAfo3B1dswI5jxAibmFkiXuCkcGxl6i0%2BhQsNxWlhuJh0Fq5NVxi3gxC3ud5gTv7bTNgJ9eDFpD0K1G0cBtyqlw2hnDixkQsin2EtGVqDSjEyJB8kftHOI2AG3LHkBt2AgxVUJmOEgpEvkV8IW1JiX%2F3cIa&X-Amz-Signature=d3e9c6485a1cd27d42afe0b6f5d01a0d620ca41ce455acee89866afbef82cf45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

