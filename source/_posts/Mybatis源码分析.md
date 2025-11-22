---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPAAFERV%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIFtGmvv%2FaM6pnNaS5hw%2FUVv5xajRjqlulXmZqnwCCA01AiEAtWAXgmyc2z4VPIgfqP5m%2F9X4oE2uFxKFHcNZ7iUecMYq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDJXq1H%2Bh5w7AhYbJwircA6JCpv6yEx8kWSw1u0CPzVbEEOs98NRMQKh8%2FmI%2B%2F2coBqx7aU8o8pAlvUyVSn0GfK49%2BRGu353U8Vx8MFUNNlyi6SYe7DCO43K0G6IKQMkF2K32a8H26hiSUfTHdU0ty7JuCG4uAVNheKnN9KLaB5wXCGrVkKuWwoNROqLVCSSuutfp9MluMy6Q5Fa%2BiwjU2SRduREMtAw0g9MiOJGJml2YCllKPwGsOWBTh%2BbUgLS%2F%2FbxmyBIdjVnajesgFWAet0AGNn7ckO34ZUVg1O6PGUDqdTTjeIONlu0UhkZclwsO4TcorhMXB2lM%2Fi9euFqhTX4ChNjzR%2FN3K1MaHPQ1EYAyg4dFESkDwktz3%2BD21MtoPcvgaDGZtDebkrhfnNKwJh2ThjR%2BHnoHHIEeLjXz7xfDUjIl3fiVS%2FOrYN%2FUG3XlPhDuEByHNX8Ep1o3KTXqe52s1OIb4fZB21Z5l3nXwGNXT5tq6eSapBAO5%2B%2BEYD0%2F7u%2FN8FlecAOavMNa3cagDag5Ft%2FUTUHbfa4avja7Bo%2B%2BB6lmLHz1UA11L2W4oae8fQryiyYMgeH4Wjhkyhwz51xDoXQPNdgFjTKOW73zvEzGNUJ678hKGPzMji9knJoH34YQrd%2BaFWw5NWB8MKDmhMkGOqUBvqzdL9U7m9vyqnB%2FYo84CN35X5brU%2B%2Fo1O8tzUZi2tGCpSXYdlztK8RZYyjInJMT7gvTpV3YP6CNFG9yq%2FkZ1%2FlfsdjR8nQYLIH3WGVQrNy98vnJfz0rKa0X2OYxs1u6gW28a7V9%2FX%2FXM7WEHfhe9HvmucxB0HqPRZENQxbmwEtqp5Evv51FiM7bznrp%2Fe%2FM%2BQKzWQcWF%2FFI4Rf1CpazWeVPslnm&X-Amz-Signature=aaab841e12929025c35a955f41a2af790f2d74e570887154233d81a6ae29dfc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

