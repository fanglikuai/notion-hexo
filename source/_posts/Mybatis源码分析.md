---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Z6OJJD5%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBcbEbUvw6pGyxiEDLL8z7xjGUQ9Ze9Gj6bzzoQCUTjZAiEAo40uoe%2FRsiF98lr%2FymUZw5KsQPSVxYjVtQ5XdYFPFi0q%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDHI2sE3PoBJ%2Bjf229yrcA35A2YTk%2BIWNFtnPuMzZJ2RKlawuCbtWQjojf%2BsE5wNazKUHWaOVfPBIGml67IbsxfYbcmZBjYQU%2F6m1jRy9YKf%2FX2SxWif7CDjangoCKYB5WEdhi7mK%2FBmFSvw8VtOnoP%2F%2Bw6%2Fho3qox8OncUdsoXVHlbN8B7mIPgUQdwwQePKEHYX06eGCEvOD035ItJK1MLG6l2H31hJQHQsWkc0fV7z%2B1mHwOADSono66i8UvAWuso52crDUTPeA1owG9mIGzc3DPOvWsCiOS%2FSN1GEnLzJCAK2IjBK7rIAsEN4K1lA%2FXur0o7OJ8k8RnpJZhsYuxsYIoxO7lFp20atiZxHwEnUzRwQ7TRHojvHO%2FjiE5dmWdZFA04q2PaAHLdHlRlaBYGSIhrZKNaHVq1k06VjqotM11KmVGYZaVHCcK3Khwfz%2BdP0GOwOh9xRrGzU6IE2Qr8P5NDJx4BuJBedPbqkzwxBdReQYlfUmP%2FO170%2FfiHAr4Tv8oBZbTFNEVGq1M%2FARsQl5COBjoEz6OPRZl4nRX4yQXQ64jj1EVmftT8l9iETO%2FrbuY%2FsgRX88TCCDTn%2FoexY%2BfHEtmMakYwxTSw9tdBIGlpKijT63pkgF3Co5na5N5sCqNZqA5%2FeDeRvFMMTNzcYGOqUB7%2FGXl5dx%2B5EayqPSRLCvgopJf3Xe4VtUPzNtkBLAT4nONe4QyyoOgmWfyQ3bDHAFv0NNhJ1yDIeUtpGz9ldq3URkdwbcC7Yvn3XuC4vMKxracLSYz9lMgXc6rjkl1nWdVtww1sbfdLQw48T%2F9nMFsDxhdGaM9ogQFZ7H9deY%2FkSVaQAJBcwQKnkWyJtXjBMjvQtPJsK%2Fm9LJd0rhKHFxrWqG3E8S&X-Amz-Signature=de493db7e3a8f88aa4a821b8a49e18b4b340d725e5d969fbfb053879f697cb0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

