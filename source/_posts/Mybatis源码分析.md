---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUNMV3G%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIFevjg%2F7M1i3Q5niqZS6NZSaAkRHs3qYJvtjPWSGXJRLAiEAklhFipRIFIrMF6uqGEoNjMSkub6mLaPk%2FWx3kcinWv0q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDpzPmDdXWGCRGEeXCrcAyRIejwlwUhQoANlEs21AV1Whqlw%2FgE%2B7SW8n1r9LysE6uK%2FJ4KZg1WFC%2BV%2F27MOo8yPO45VLaotoa%2FiHql37edL6xJzVxR5Ok0xGOH2JJk2Dxwn3b6EMZx2lA271kscaghQs%2B4TcpTorlLhudUwSHHFAAqxmKKB823YHJ7RuLn2jPWkkybuZF4sRq74uzlKrMqP6GNTJXTLD9vfKA%2FZK4UgC%2FXc0nBU2ZTGU9preiDNekMsuaWOYY2VgPlRwHd841qMl3MQ9FymXPhNiQLdtBOyl5RNuO9PloxHQsnCWJpxdO%2FKh3x%2BvHgFYIXwdQB4bO0KJ6VI%2FMa9IRr%2Flwlh5afBgqyd6uo%2FSeOr5BcDMRXeGgqIII5lBWcZdZrh%2BkTEU0bEv%2FgYlJH1k%2BkMr1hjhXvnsAKsl47%2F5ybAlJflWt5AYiE2lVCdGxwOBjSNs8ZTuijM0QAKKi47jmQ3Xw9BxETaDv2BKTHzoq11yE7Nc%2B7FwWAtxyynP%2FBBc%2Fp4DCqirHygASTDLc2HiIosmZUsajfAr3zvTViDZHn7EA4b8ZuIggqslCmS9LvB2mpdHc%2BJj7AP1p76SYTEzFe8RK%2FKcQB0aMnETGDyd%2F6sxLaUhd3Azh48ja7smyvgr%2BNgMLSPiskGOqUBGydC9RPiYlGNn4hUUaNw7u7ieq0HpGhEX18GX0u7rLinA2PWCb3gF9vKBUuxVKKpdTVYjsRrUpY9B%2B2QaOWlemPSY6g6DwiAUc25pp7OrvqS0R3G7TJqjzWsj3aL%2BupIFO3D0bTTyvqpa0ajwNAyUIXkp0RkJWAuvEI%2FfyPw8kAVFKfX0aa%2F8HFl49ViqWB1ylJBAbAK%2Fp74mJLJaisVsjcmcWSf&X-Amz-Signature=418aba4623b17269eaff25df5e44267c0f98e8831170a68f7411b450bb84ff11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

