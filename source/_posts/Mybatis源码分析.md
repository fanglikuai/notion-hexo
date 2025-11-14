---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CGAHXDW%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCCtBfi4%2BHC2YOHqBTV3LsqCObGIhQqFTC62LnY6UoYrQIgTsiw9LG%2F04Rle7GqUCggeXq3UkbwcD9mZpoJlhMDtRAq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDGEjgCVrtSvOU0DDIircA5dZO96TvL8ntKR%2BoBokIr8ViFlecAmzm%2BtG2rUz%2BIZPkJ%2Berr9CP%2FL1WWpj0w3SAWzIUpeUMv%2BZrNeaONmwrSHKzc3iPCWk86m9iQvkvVnURp2Dos4AbDmPey9QwsRe2dLNUVs0c8VI74GFiSpI%2FQxcPWF%2B5vBYedNtufYOYgreL4Yk4ZI1yunD2rVhF%2BTwcrbaRiLC8Q7lSsCHWOK6yPtEhQAd13Cm%2BAwO4PDBIn5pjiS8RLVrz0VQhFyzoyt%2F74ehF71PssDiCaTHPSefPPlONEEU1Mts9rmZrCuXX%2B3RwnoEJmkDt7dpPLu9Q7dtSTiMvu6iostpeURX%2FiiJhSTbHjnEihgaIbNmKNOhjLf4sQizt1e%2F%2BY4l%2BMLr6syx%2BOkizcs8g1Wfp0F4y73cnIK00olYUzwMRNy4m4nntoFJsvPsFKpCdvFjtrQUHHg%2BgT4Gxo3hBbbxALqix5kVFiwkO2RrvOv3uRSGHq%2BqP5BwLiL8HiIdpgYdGIjcH0GLF1dKEIjz7r6OniGnRu0FFJXuAatIGmwhfP992jgPTrHfbefHoBwJ%2FCk32CC94ukI0Oby7u054tH6shE0FZ4xY85B9%2Fambqax7UZ%2Brt0L2E6UNxiUobxi48nvU16bMLjf28gGOqUBelnFTWAAgN1vUJ4c2ZeJVCCyNRNUs6FuyBWIKxRj%2FO4QEpPZOcHwWzhmhE42rTWhnQ9ciLVySsejsRlunTkav4woIvSmauP2d52K9LiND4pJzPFp3lcRiQenfbY1BOtxty2M%2BsGSIQRUOZxF8INyDftsh8b%2Bkxv9s6E%2FIkELLNpvb9zEM3wH9bdSONbqJIFiaVKCbzLRjQ%2FFPk%2F9udFEx29Ug%2Bi6&X-Amz-Signature=a18e3d1371468140a77cf3d5eeb37e6797051c5f71a5d3ecc7a01575985a2a49&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

