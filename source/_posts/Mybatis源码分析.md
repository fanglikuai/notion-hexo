---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RGAFW2WY%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T170039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCk8K9cGydvFFRMNyhxLmFwptgNLx1bCYbYEguPlzpJUwIgATMDYKOnagcYmTTIIMcU2zMzrwOGizQ30zrlJ4Hnz98q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDN0DNlghVrF10AxQSircAw%2BLW3I5Je65jLkJC6%2FIJUugdjU4OeyYt2cMpLW2Oag7%2F1m1XPAfptWW8Zqi985KymgDrexOzRBYwFmBcJZeq%2BsWWabp4g%2F%2Fd7Xo29ZxW5OGxxQ2qPZ5FShDCuA8aM5pWRn5fVK13hQzDefBTZyLG8W0lVY793m2LtexWKs2f%2FSk4SIcQzsxwhLhScy4kfb5Y4iiBnhcEnBBLsqUaffaWTGHdQBbxVwtDR4X%2B89T7mRtzVU8egUaRDEfWIcyhi%2BnIzxmLawDD0sKS3s8mJa3yBlf4DpnCWj%2F7sTPkaXo7ehkFzrLMO%2FcoR8YODdol2bMHf4gPllnr%2BA6yYMpjVt8NiuvAtex8Kjaw5cKCjo2QCiV3Kw5eojAKXqoHOFZuaT7CAlC6slXEb065FO6ugsyQbB0LkH9%2BBIWgPukYiQktxVfnLdAVb90wBGRGgZPzPz6y4RDV%2F4PQlfGBxascZEgBbdtFnwE222uxxIWlfndBTiixjL8Gww3SZI88MO0JW0jfzv4WSnt04kcUU2kOWw49rGKDtsIM1YeyUOC1rfFuMS08QQGyAmshE%2BGKGDMde0a2U4eEbBKOR8LRKuMjwZNmhnKN89ZiRo13OSBP7QQ1tAiZQKroHgdSRvdUegwMPe71cYGOqUBRm7Y5QiFGvhrd1oJA13Kb1yDNmIHt1QiN0Nu9YYiTMZBzovbUi5Nn9e%2F2Mf5qLSIFj%2FKAxlBh21cWVFrlrmS5kJF86Vp2AT22IJX90Bm%2B47FQ196je01RPV281t2ejsYSigTiC9fPl4aTSvuwGeQ5uk8687gMZ5Z7zi9fyzor7SGKgL4wZ3uukoiBx%2F7o%2BnuGwdLPVgzFezd0a1nP1XOLEiN2vPw&X-Amz-Signature=7e37572a7d5488be859746953c39a9bf22367a995f36e994d6e0b3ef82162253&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

