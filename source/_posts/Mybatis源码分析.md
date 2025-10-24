---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMU5JOSG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGUIVHpbC0KEbavg3wUgR5edQ5jDnMC%2FS%2F22jULbu4OkAiBT8K923XvmV%2FCVahd5Xk5ZWRqHCQLGcZOUKBVMgXRH3yr%2FAwhlEAAaDDYzNzQyMzE4MzgwNSIME9aBiHrsW5VnTzC7KtwDba28hA4LjD1JLFrGswDeMZ9E5%2BrPqT0b4mko6SgFtTQVbhGoYDgRCfMtGUfA93Pk1CWE2nolHJou1a4LV5vFQRn%2B38slpUDLxY8TP2vPDV76gOWCTFuAqIsC6WaStEW9mdgrbrFlqJJcdorFZC%2B5qNHWct3YQYhBSiIXs33AbaGmmBpA1tO9JISpyIpPb0MUPAr7n21SHn9R%2BGk7f2DNs%2Bu816%2B5iqAqQAgkmF0rsBLKarmEk9UR%2FcU3wT1i4pk69LsIMlqzxEv1rcjqi3Pt7Rw0EgLMfD%2FM%2FSHwmmljakRcMTvfatuKsdZArV7TScdcrmN9FFhfUBg54EUQ98HL1pgoibDC6Jo7PVB59YnzBzOpCEKfQOdnlZqnwMzqj5XE5NXxZrMyxjJpuHGA2BP8RTqHZIUz6O9OkGbu5ECv%2Bujjt48ET8OCUjM8fusp9e0hl0DE2v8v5M0Cdy6JjwlkELNOxXVjnkA1l6JXfs7UHjmc0Cfv7MSoqQ739o9eXowkjTmxuGcdyyL8smpvs8oKAb0dH2Ekih8c%2FcYtwlBJozM%2BGYrcTERs6wR6%2FUNNzgCPylij3o%2FljpwR4N5TMlkJTJ1Ji7hvQqjPJP27QLNL0ASzGpAG5KpiULBpOfowqLjvxwY6pgE7fRAsqRWEo2iSA6CaJ8NDAXsoN%2BKYsuhhxLUhdUPDZelVbh6%2FZ5P2jLMojh4ZHrbszseZ1bF79bPU5%2BgMySDo0dlSf7E9qgQLnLHWwWnv7n7otqEloW8NeJvVJEsQBAEp96yc8bbVG%2FyUmNehcrLMEXQEPqc3pa2MPDPEnWTV9pJ2bneUFPvwJVNvnnw%2Bs3%2FFeLk7Z4keEfQg80AN8J678E5ewvuy&X-Amz-Signature=b62f7a68a1ce3f9cf685e43b589e3ed6eec9e6445298ae8132de26464cd87e00&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

