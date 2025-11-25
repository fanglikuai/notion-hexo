---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ZPEOUIV%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCI0BSyGKOSo5tKQVGaE6yMSRtLDc2u4M6uoscsaFJfxwIgBKbISRGICGbY%2FctFDAoE2mbikbIM5xzahR%2BOvXjGGVsq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDEjJQlduw%2Fiz1BuyoSrcA3MSzaNUNCWh5Qsert%2BZI%2FlTVQm8Nc6NvBnGVHZnwJZj30vYiX2yciwkW%2FNBAnCk9M%2BOj2V1wTxb%2BMfp3rFWjEKnoESMAEkX5OFi9%2BbefEdQMOArt1kEAU0uuOvwcDXk81L0hZlL%2BOnsr2LNHKTUPFm8kM1NMyYbKMsNIcui2WGEGa0psTUnd0DhD%2Fk2kmMbedleo%2BT2AXTzgwMcNkm64W5FDdrpQ9RPULYTJw5ZrLiXRhD5OwwegSj9MhcK0no%2FERk0cTSwehNlyGBNAbPqfNKxMXgY2s%2FaR6ibAq01kJKBoB26XyYLD1xYfhUrEowK6yN7wfPbj7mVuO9ugYVB1ebW4C2GG8EW37bqv8Ss%2BsqK1div41SA7D2a2Cpo66N4FnH6sdmxpVOKlIRoH%2B0BQbA1H%2BdcHE2HbPrnVib3Sz%2BcJ5DKOGr%2FjvsocpccV2EB3q46hPaxCNICaNJD1Dw9qt4Tz2QEFy13QM9%2BMy8ENrwtDqkvtxCpjQ04RkS5frk%2Fzzi6B3waDF7cvLdNdZaQfDqRxjtDlzHipJFx%2B9kuhVlQkZ3tRZDCE4WX%2F9QwJA3zrHob%2F8%2Bcd2q1pBZrkN7zlBysqoTbogCl7B6But%2Fb30TUI1s%2BHHZ%2FdBU0Ebm6MJaalMkGOqUBaVdl%2BrLW7DV1gJM0JohCUyOMF5qCsW0j3t%2FJ1ENr0z0znIZwI97IlzeDCtAni8%2Fi%2BxX4icxKPgzrcPgAhP5JczBNJwXEQOUPznDOW7v6yaSNClqUg8ZRqUCbFOz%2BLsFBKCTUIWlq5VHgVnFA4XddZqnw7Q8DB02fNy%2FvNPcqP%2F8wsj6q9emelhBsTM3YwT5KyJTizsaIaOn5DOE%2F%2BCHnD9Zh8mKF&X-Amz-Signature=d84ec02d0c35f66eff279944d7aeb57183212e4cdd900fd8bfddefcec5b4146a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

