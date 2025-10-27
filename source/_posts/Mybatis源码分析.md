---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVI63XWM%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEyvupk1v8RVL7nteY1l2xe7%2BEY%2B%2BHG%2FpMY4FIHoUOryAiBpfCY%2BfihUhyZMm8ayctmX4SnlSla6bsQqc9WkD%2FDRUyqIBAid%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaxQRgVSAbeUWpNjyKtwD%2FM%2B6AUJv17ieGCf8Ldvuj2fG%2BijoEQs2ZNg2HQMFglhyKwODmPSVUoWJQ%2FasbAjTYr1V3S7t90By%2FPab6OyYaD1gwhKTTH34Jq6ddc%2FLPB0kYSxNfN0qisI%2BvzZxOrI59y%2FRbFWW77KLOIdINeDV5InCRvjIpq%2Fx4k0k3%2BSmDPGwlHA79qC73wQ7oGbP62ja%2BV3duG3v09kzRt94mn%2BmJvkF37HqDbbuyyGZTIIJ5X2VXKUBPIzGuz%2BGp27op1xmoA6sZyV3BJM7hL48pwm9Hex3NIZuxwzq7BkoydwjImDAvaakr0Yhm9Bot8BdLw3dgTePejEKeo8J%2FBGCtt%2FMyXXDnTVxSScyPUxD67V8vs4HFbgGmJpHeXZy0c45Ifkl3jQcIs7%2FdHl27SzXbchS9HDMIZ%2FI%2FRmbF%2Bo04RZM%2FkkkvJLF1fNYhEhpTqQ8wduU1vcVF1GRL%2FMcYwenMiP%2FN28l8eE%2FuJWrpIgL7%2BAJ1lSqhFCuZO3w58mJG510Zx3MupPD1Rq%2B1OLTJZ2NBO3vHjLZE21frJR%2FxMvAkThbPxxJJnW2oxdgdV8jWO3xiGh%2BzJB2Uv93R56BTD0%2BpZ3SCR3p09YYKmCxnwS4vEr5eSEd5b3cBDveTZl356ww5837xwY6pgGx3gDCD%2F0%2F4T9uIT5KD%2FViP%2Bf1xklt76Sxcbi94IoODBrfo5pNPFTQPMo7pEGE3G3d5aBWzsttAYYY0qRGv9uoZftVQbsI71SfdrUSWCtqR6%2Fqrj8qqOlzCPLTS7oJeo%2Bh4%2Ft1Nyomm3gTMEJUtqVqNkODWBliNE7dZ73%2BfOHsrWY67ILDLgLJW9rC4u0%2BALKuhpiL79mAGKGt99F0wM8FHVT%2BRzxC&X-Amz-Signature=30b37e68c92afe2b8fad3a6f809dad426010f59fbc9bb8034410510dcc13ac17&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

