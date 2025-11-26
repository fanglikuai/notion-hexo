---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W75P42TO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGmMlaf9u3hMIsQVZrLc%2B%2BcmPypr9Wl70O2eSeCDcqHqAiEA%2FFJcTMSgR9XRNtcFHZBgWCIR1t2Nw08DY5dJEXlDduEqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHGbYm4eMf5JhDrsgircAwmgdsKUfz2sByb3KMyfPO6GCHH6alXc%2Bk0b8FktR%2FMCJbHOwMDjg8uad%2FmA3qyoXknJj29Wo2HmJFkokpowwK5pMBdOyBQAlHvpZv6KyYB9PHwDc0H92m3v8aneysVxqAcIkcr6YSG%2FM0VWBiJ5jMfl2uVFCVgM1y%2BZQol4CjxTJ0KOlCx8XB7fn0uBskPG2vts8alxHcn9pwcflj%2FH0h2R6Qkwb3IQ%2BIILo%2BeaUNiwxM787oZZo5RQEt7fkp83NUo7SIrGcWn5N9JGr3%2BWK9EK6Ln%2BGPKtxptgb57vlmBriUfa6D6s96DRhCcNFeHYN3mGFD7qSL9L2WLr6hciJDgsydcT8r7PDtxyeEN4q5Yoj06Hj2ug3CEADx176KdaOl5tv5y1jWfNG3McbieSNgi8L4YglANhPItC3MN9t4r3ITY23iYXMVUOcctzJelxLImHT%2Bc32%2F5AkL4x6WiwfZlVriLiUEm6q5tj5EWA7RutOjZ0YQ4gwkA4fl98hy60E05fZBXd17vWRCSEB%2FbXBCaVBW886gOWUmKuONOcYKr5BrHvJpZ09nmi7tnx%2FQGwSb%2FYYQ%2Br4oXa9j8B0rgF%2FAZxd7KafLA0mqbjic17dT2BOtZGNkeD8J4i%2FYzWMI%2FqnMkGOqUBc6B4viJEF1yLURtxJ9ASiASwZHCAEYTCVF%2BJkisrtXzLsH5t1Vsmq46mqKhTXzyJpxrletV2R5aWEp3Q%2BQQ1GUWlFFnjHvRV7SmcBO62LLvXKN3KVI0mRlYsiSxuO7BuOrUlzvkZ3aupF8Rlz1ur7rk7tkPnTedLc9BRN5h3X2BlFv7CxbR1jqQWrlrAT3QtWSnswcDDTDAUvTj3IO5k6n9yDqhC&X-Amz-Signature=47438c879354b0dd439941fae7903ff40c8239238d15ee2da2167d4092fd82f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

