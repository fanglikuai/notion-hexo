---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4F2WGDS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDyeawybrtxFv9Xrrga1yUc6yZbF7TTmqf6ITFQO35IIwIgYKqhEwtETAsAgTjvaGEPPOWaZ4OwPsut%2BCjvVLOVc10q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDMBoGnA7e9La0MiR3ircAzi5VvL%2FShtM3j%2BOXg7VM%2Fr32WDXy0LXFkjIE3WWg0KrleqJ0cZ23YIyFjTIhomn3ZtMDS5eZv%2FvmKC9l0734Ynx%2FOBWmbp5pDYuLxFk0idbIiRYlK8QZMJjUf87XQHsYJEF24YntWtetlW8N1J16QVkfHaRUNpIOXP9YWxQgJgwXf24UVtWhaU9qvMFS2Fc53y9O0JmL6lyzKrD1x8fsRxRzLUJgxqd8zTdv9HPiplGmUkv3rfr3g7XtOsK8Dm%2BjHtApQLgHVw8vrySL8cka1QCeUv5SJfM%2Bmn2bODJT2CS7EgAFJuj%2FOm5sj6OSMxHQOcxuEFhnlKErLCuz3dbVHgCn1b1ujoZrnnqhIFN%2FIlFFjmKC2RAZmyOJuCbEVAva4Jsx9v0NcBpoaLw%2BPzKGUvO%2BRTY7mW2CoU2jfZMUTfMVTTfa%2F52vWm9IlGrLPegUt4Nupzn2%2BoG2WaYE9QvmVdjy0X5vvujXiQqAAE0he3A%2BXznyi94HGH7UAZigN0kYVk%2Bpt0s30XdJtnMtOeVYLdkeZvIBgdpmVE1vr2jpmZUY0dm%2FBUEi3181ZxnkMzdCvdQI5EUa%2BpiJ72X6GtLen8zLXSf8T79U1xymi3VlcyCBVkLhQHxWKepng3lMMamq8cGOqUBY00%2FDUSuLp9NYaw4Sms2Zt3Nqf41uazEenOmNkHg4s1NTFaUW1lBDTeV1PQrObWMjInED0g1JONQR9v%2FoEQeM7DvFexdB9GvbE43OVBo1p5Vc34i%2F2k1aOPTY3x4z0VY7UtDzwCPxyuTw9hw5RM5V6yOapK5ITz9UjKvscnevwYDWSQjvvdIFkBe%2BArWl3Uveiy%2ButYS998umjcwGr9hrsc6zEmZ&X-Amz-Signature=c93377bc2bb300d72f6a05a5bb749ccfb8d55df8fbfb0273400fe4d161dfbc75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

