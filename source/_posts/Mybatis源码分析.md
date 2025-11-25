---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662A2PJABS%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3xE9fXcRHM7AKjT7m4MdYiGBcdGlixq9FsCdTpuvF%2FwIgBj%2BZ59UqtQ8MYANSaKlwXp%2B7XIXClM%2Fu2HTfp0t6Vk0q%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDEtwnwJ%2FK4RGuSYNHyrcA1lCXQPuptKlS79EZqDoHjod0X04ktwGnRDsFq8pftXj4BuyrzXY9%2BHdXoW5pGaKktzRJCTHvDZtw4szX5izaRDktqUUNIiAAQr1DBeWZU76ue09UtERVSVEhW6b%2FafPbowCLEyh2x9qJMNMHoX0udiIlehafS6VAb5jOG5CfDmEhIvuFH2bN%2FpKbCerbady3GcGZOQ%2FYZErEJvFRTZ4UoreCdpXP672VZ50gWmhbE923xYIGNKWppvvXJ6bZvrl0TNx9KPVAi%2FvvQ0Zmv%2BBGf9TiU6Wzgm3o%2Fcv0IACAsFaIzEWH6vETZ4z6sKDDa2zzr4LPNiCiGWt7rhXHE%2F6ajFL2%2BJMugGZA6Qh9zDcyIlzsfw82heL4pASQKfAdw9gRXdXVqZrvRRGXZ85lPZ2XfqhpRIPFBsOMuVCexxTJ3hv1Pw4ZLe4cTEmeTIjF32tP0thLsPBXpng%2FOZ0WmJgmhrWpW1fYmO83Qgr8KAkxwpfYadYQEt4UTLovAfniANh7Rme%2FQCLGgwUadG5N1R5mvwzwQ4jpWb9nwKNrdh0FJK4BMSsmM88ZAKeMp1X5YnTEgJN%2Bl2%2BwjEpsol2Acqc4GrAFDMTyz22hyUMJVb5HWDKZpvSt%2BKlycqe9wkdMKXWl8kGOqUBlTZzGVa8tFPA3Q6rX1eWimTbwD8gLAFeqxbS2TeufNvO9lZIipVtlWkXiHQXI8quxPr2sHCjiSKXYYO7s1T1CSiGKhEUkulYBdP%2F33kMCaMT1XasMQCMkce%2FwJpdAvhb1e9WzQDl88B1STNwZCCOZTbO435E%2BM0z8V8jVtA5yzR7dE2hcjaDepXWCJQtBSH8UHlEayyENWbtH1MIGdTQ6dWGuPey&X-Amz-Signature=1d263c48510ed320a571b6034f1c8e7a6f9de4ca97f1075f2a866a098f710646&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

