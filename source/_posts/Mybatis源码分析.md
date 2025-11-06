---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PLQZFZ6%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkpprREjZgcT%2BIMWFKaHTw2HZ70cBRMmpXO7ykwQ%2BJfgIhAO5NVTcIxkJqwimjfF695Vlgsr0Ucg6u0LuI%2Fad%2FxBa3KogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxAWB0Gc%2Bs61VUCR%2BYq3ANIGLqp%2Bf0bMKwRzUlxZHIa9tPx4jpDVCb%2FXeoS7W0r39HBKX2VxtV9Ahffd2n31MBzMkcOd6zU1%2FKAn4KSBApqUQ%2FXJhzDBBqxJoSXI925sQ0GFRQ1wqOJVrHeVNSEdCCa9qeJNg5oDsNfHOH%2BZqt8mASXlLkjitJSPlfPjIpL8MyD7494xMQx1d7xEfM3bj0sBudVI%2B6%2BEfiafhp79N%2ByChMlC6Gpn1tDggyN8Dd1h3KelWkhR6x36dCaRuAINkq8OBRoYfDZLhWvrDCqn%2B9PrRjBISufLU%2BagmeNsBNjsvTqzDzlJdjSkGOvrtZ3Pj9vYlIiau1Gxw5ZjwdTbAsyU%2FEs%2FxRwHYcUkwVWh1y359BL53Sfr2jrxiYV0SabFELIcBaE1g00zIxk7VsHEdoPUs3X%2B0cNAyWvPo9M1UK46telXz0qu1KJZc1OUzfP0FGOCYdNNdbXiSn8nmPgOM6OZrro%2B%2FThZzH%2BFPLDSVPSpKFZvwi3H8d3ywkR4eFzdqjBdOWn3g%2FNwGTPNcHXTxVv5klX4WiSuLD%2FOIyKEUoAKlkh8Ozo5L31MFr4dFQv30qHIT%2BbcsdyftbWfrKSA8gq0mtP%2FEBx3UXMxlGBAkbrjNSLaGpra0ofPCjgGjCpw7PIBjqkASD7F1c54Xze3ja9xoSsRduTkEHYklrM36oi%2Bw277sSWo%2Far2qmnOi9myxoHOs6Wubo6gncQbr00XoEe9u9qSqeuJ4li5lhw%2FnjYvNvVNiiu3OF%2Bcs%2F3bMm3CHkUI6GE7nEUpqEeAh9P1ybnVUY6kfojXPe9Tc2sR549rIRl5v7dMaLgaEZr%2FZZkgtJbJFI%2FXHRFYVCU5lA%2BxlvgR7J2uaQkaTXX&X-Amz-Signature=3e1a54a0cc24a9fd6a5ada6363328109d9971597040551055191c24787ea2ab7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

