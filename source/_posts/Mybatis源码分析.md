---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE66G5RS%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCAhm8RLrW9pldjIvBGuWfUZz3ojZU8tGAA09o7bwQ0QQIhAJ%2FQD4D2%2B0AlcoG%2Bc6kAtUOkeydri593g4vFrpSlFVN7Kv8DCHoQABoMNjM3NDIzMTgzODA1IgwNTFXtKvEqssW6a%2FAq3AP1ns3q%2FIYtSvz0KmzwBgu%2BV7gwwOJg4NHU%2FypbgaBHNc5oCBU2g3rO%2FHqOc%2B1m%2B94n6kedt9TptqJ470pNNbtocdqhMMuBQQlb1Cs9IEOMHqRisMShRULM62bsK8Lk2Zbc1kimQBORMuOfjRS3FHqYu0Rfzcm86bancI03YqBBh0v2j%2Fpc%2FgNCmJjaqSVQcA5%2BsDHadk2Pc5b6WH8b2XihkqNEJRySXa2iYo05MAp1m9y1gYFyfcjy6vmyT7x%2FpPm3RSloAKqTzus4An1Z7xmVzLazLQUFVWMOImDrBI2CZpYSmc70hgA4uTlXsJkwawF67LIMgQ5DRfYx0RQmhYLS6f1AA8fVCnmGRf5VovtuP54njNoUle2mn%2BQaBTf%2F40ktL%2Fk4QsLH%2FPqSmns9f%2BG78zTGDPH%2FDYcbuTvAkCIurOrX0tBSq4SYt15pCMPoUR7I8tzq0Y24dt6KGiFMYwIuCX7MuzOJR1ofQJv2xzb28xx0EMNfIWjd3iCuAqeXE7k9c9juAYXza5D8Vy%2F2%2BFJL3Y%2FSJizys9x63BPwnO0tWer%2BrYL6E1S3%2FTDAfUOUtxF0YVXLsC9yNlgClnMqAORf1HJbZrezloYGzitVocwPJS3MeZE6vJdVDaNc1zChg%2BHIBjqkAaT1LKNsIbi%2FKC%2F8%2F5058h%2BqigRi19DWq32V5yoOdpAfO8CdM7VfhmFUrxG%2Bn4PCZtETOt%2Fx%2BYDF4o20drclVHyf4VUIRo7j2Sui039ot5zQcbjUbG9t7l1Iu28RZ0Cwlj62o%2BKe5NAZC%2BTHLKb8wOSf9lN9%2F8zvo38YuV%2BUVA0xSNdmOZZsCeZhabVmcpSaAE5517Qxo9ZHMzfa%2BgAFxVDfwgyt&X-Amz-Signature=6fcd2145ffd9446466030aef7fa3b7b3c7f33eb3fdf394d4e77dbdec2a81e490&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

