---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMJ73YHT%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDA4Lb53K1hwd6Eo6Z5YbKBtH5n%2BZlVA3XEttHRsNCbnQIgX0GcWCI3%2BfvhYJ49mwzvc00RbHF317pIP6fkRafwRx4q%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDCMI%2BaCR74O0mCUL3SrcA6M2F7o259cGLLc6884U%2FjPfifTOc8aV1sBd9tkNGZJS40IJWH3NGFxc5P8caWb8IeQitTE2oI1t1N1GvPhtvif%2BhYc06KNoFsCuJIm5SVg43Dc0xJLoe3HcD0t6a8RG29vHnkLG286KQfjizRfo8hiiAJEgWSDPEuhWVdL21nbUP49e%2BQXvhHDJhmPt0E3JY4hSPIzALmgJ87m228zFTE9IyTApWbYkb69xp18KRt8e3dWu9q%2B8So5DIjW9%2Fy47O6GDY2yFf%2BpybRw5hibAIyevTPO0lkfkskiHxf1V1X%2BxleipGPCG3y0hPJTgOq%2F7Xgr8LUEeb9FJArwRemv3abT98IhoSusllvjO51MKun6KGk21zSXkRKVo6xvvAevQx%2FCiKG%2BYyUtgho2oXnUnB3cWlCMckgI0AU1ggz7xRAfBhZigrCGhdLjy2WD5qUJS2Yr9Mxg%2FyrukTRtMisnsOesN4cGuJ1rVZsns8M9A0OKftXIfP3q3RxMCYgNIqTPiTu4yky1ryxJhNF0S46etmF36A6jAZu6PDDrmEnioasD4jbTS%2F%2BoNCzH0vi6pTLWL66rKy%2BhZn8vOEa2JjDQrMIo2DB%2Fdt05APpfb2JvLeQr7FbcEuX8s8es8Ifu2MLecg8cGOqUB7Vr%2FLE%2BDAkO42QqEmutV9duwF4hvBnAi0ZXUpbjf6LQmJg6XqE4Xitq5mRT4DsQO%2FMWCioW4jI8x%2F0DrDJe1D4dEXri%2BOg2PtIcNZp7b1%2F5DuJKZWpIp5zQb219tzJqFvqGLOFFwj7GqT0e44O0mxBpcIgThVaUFlPpnM9arH5aut2xz3soM0yyvEnaWJjMXATBNhRmm1ieHD%2Fa32eBYgBBHRgNs&X-Amz-Signature=5e9e9214de3ced3d6dfd235cd91ad9c24f6197899354d042bb0faee2aae4bff6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

