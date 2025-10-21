---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46635FA3UU2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDQbhmqcso3nYsL1riEDTSnKlDfjDWsIiLkoQ%2FFHnQUNAIgccA0ieHjPZDTC79f0mbIDj7O%2FPqINkvDHfi%2FJe6w4SAqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA2jWe8LD61fPLXvrSrcA5qEA2WJDGtL0AxtEDEqOMEgeXHXXsxl%2Bnm8%2F2IjBdbEQtC1GXByY5%2BwavLpup6yScZdgGoMlAlhzMjmH6wrTH48eLaVZXvEYJWQHo%2BiGHi6qATCBKldYmbpoD9bCrS6nE4xUwZtawA7cXkc1nIlTbwavQNSP%2BJJcIkN1qXjg35ipXB14CIp0T76XHJPa%2BxUODo6Nlc8li4MMIK9OfGJV%2F2yggGQ7Od6hiyzZOu55zdXv1Blgyn6fnB4znHf9UqA36QXBGhhxugCD%2FQc1D2dZNQMkrpfZy6mJhji%2FknnO9gDzvCwZy7vRWqpsQPnloFYr%2F0j0CoT%2FHm03nOsrybLN1iA%2Bj%2BkswgFpvHed6s6wqDFaKHZ9%2B2K8bDGgPWZb6kY%2BK%2FUIOeQ%2FwdUtGqhdxpBErmzbaSOSsh8YQHaklkZN%2B5wvLM3hJCmivXlORlvWqIKmgjHn3mbAUDJ%2FrPOZ9a4zqz0K2a95TIDSftqqz7JgZSzE4HMJXut%2B0VrZbC2Y3wOTcOApiBDTLVpY7mYnAxw54f%2Bk1rhNnmMkraocGX9Aw9O6HyTyU%2BsNzDZ9SXLrTETd%2FmmOdkZJWs7rw0iKGvyFxIMN1O7XM7t%2FEbQNJ08UgOdngDfqJePwmkxtOS5MISs3McGOqUB%2B8yVZRA5cyeLZw0FuIifcuN9aufYOYw%2BKbDr2OT9tGU4rKFGBWV%2BLPrjH7FN8Sy2jRfuRS9xUHZsrbNcsTFfJpoIIdbKWCENECLppKVOzNbgXCgnJsF1Ic5W0acTZoZaIlJezW2rT6Whgdng4%2Bojb4%2FTAkE954m8wXDKcYyO%2BIlBznAZHi5BsSbViimUL3kMvEbSAtxb9oTQp02QMI0PPQcT1RPs&X-Amz-Signature=741a2426c57a5c7f9f879150e7d81d35c51d3dae3d018789fe50d52f43dca1e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

