---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3R6K5FT%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCICK%2FeuFlPXS5r2wAaUb1Vq%2BDTGM0Jt%2B1%2FMKHlA2xe2h%2FAiEAmE%2F666PTEA7JFyrq8W94b2MNMtdGkxbt5mfw4227Io0qiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDImjtVKN0qgI%2FSuDkyrcA4XvqSumICawkTXwf%2F%2BcWCVGdYwdhbGo2DJTb%2F%2BJGjH0V%2BoXe8r4G7IccnQWOCeiNRsLL7wksW8wxXMA1LY9q6kcqyqvyxndF14Frd4iHneUk9o6n1lF9nNMV29s%2BhOHzjYS9b1ZXvl6SNuKZ73nS1bV%2FbpRM%2BVkHlOsznngHwm8KTezw5n5LgLMIimKuA8nHifDxTh1QSQc3QPfzpTEHilljuz5ac947H%2F22zwnImID%2BYnOeyj4TQI8uHQUgBOYQwlRcsQizPwkf%2Fm78aTJzTki2c0QYkOBc7ucY42JVMI%2BxIy0yQRAVaHU6ez8sld00gFXwSFKnc07Ew5Ms4dLQIFqA1ppCRfTxw4XuCa8FgEWnZHSm%2F8AtDNki3a%2F0fQ4o%2B9Bc4trzzocglstIgj6Kr555dZuToTnJMzCKf3GBvnRxDR%2F0qiK6khFtZFn7RyK85d79UydOe4U5UwB1LU9GImvCLjyqNMgsm5mtUu20%2FpzW2eRY9j9%2F93yYax5F0kvjkmp0D8lKAlC9ySWS8bEqRD8YSAGAqRM9Co0ZR4ehgywbOUX%2FUmse%2F0n%2BIoUJRJUSbg7e9sszbq7KpNsstjYnkLZRlC%2BHzFUWr7ayNgXCH%2BummAfzx2sMKP%2F5UbDMMHq8cYGOqUBrVvxRCz9DVxAa0T7f4eO5J7nYe%2BEHt1icM9snI0Sa4xsQoz9Ichx%2FkhQ55wdlAkKegkNnKuMefkU1uwZj89ECNUB%2F7dc%2BygP%2BWbNqm%2FE%2B3rlUJaTB4MMZ3lSz2IUhwR2YNoKebFkZpoGeMRVFiPJrCxBNBVSB7AR%2FT0U2iM08b%2BjC8Ng71f2psUbRTnDFeRiyBkzdKinNatCQoOm0B8K3h8%2FTxxK&X-Amz-Signature=9b2cda9bce36f7e14787e02cfc8f590e4a2de1bfbd0392b1bb41380ae03eec50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

