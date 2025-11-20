---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VC4J4R44%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T080051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQC%2BU%2BuoAhejYhQ1NLUl2ydVg0ZjLPL42e2RDB8865ysswIgPx7Ltrtmyn4BwH23TO39s4TV1RQYcIgxLfuZttHJVI8qiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKwOq2VtZlxU9WCw7SrcA3Dczt7oCkq5aSRON1nX8yhv18XrFNrEydO3FPgQZjTFPeybBH0yFyLdOI%2BZAYumleBZAwfbUS84GLjxnNNe6IFvsyP6ayj3YUjLTY8ExyojsXSZaUo732N1iIU2Tkpr8ku1cBvADLsJShpvfwtClpV4M2J8EAOJ6DNuWltM6luNMhk4Ke%2BeSIe4RIK7Jg7uuUiuoTbpJs%2FauzQj0n81TE0WwVlTyyPlMhgzC%2F%2FZ2fyEvRbiRI1ADHr7hAQrCOsI55vOxrvE4MF7nXEBI2NcgXPd%2Ft9HjahVkU9u9cauotJ%2Ftr1%2BH7PZ0G%2FNJoLHdeMRL68sb0NqH8McHW%2BArtF%2BwwgZPCRd39vUJ1uAANiPEio52%2B%2B1uySWIrb%2FYwCT1G7oZzpXTLwdznu%2Fc7c5KW8WQlgUfIDxr97j3zjqx8XkbtWgnxxI5BbQVjigkpMZEg9ugDL60tuH4ygwPEX6X5jBxeuJnCpKDNaej3bGEKjvBHVaiAyfUA6XS9K75Hvjxln4qh2bvni%2BXb4a4fFKpxh15JVuXoVyBYVJz0Tqr%2B%2BkSkVVbwwbuylQjwrOJA9y9O60QrTYballsk%2Bzfk9dtDZLQm%2Bghs6Dwt7MnE%2BxqCPgO7TvtOrhfJ9CgH42Q6WRMPeS%2B8gGOqUB9lN9wO6VBrCudpzN3cPB%2FzMmkfW4CTnpG%2Ft33XWZeAh55O4aWRNiP76nuKDB%2FU99QGkrTC6XjnAbRwTuSxYfVDKgPCxnl3c%2FUm23GsoASl6oPehR30YGm%2B7wwXMusEtKOblBk4J2Bul041ga%2BTwqAaiyodEV9QBvfkhc%2FjSOkM8NJv9Fw8224uKeUp3w0Mq%2BYcZ%2FjwFW9sZBtGD0tvEn%2FrzT1BTk&X-Amz-Signature=80752cea879b48d1919becb586ba0208f6cfce3a48d7b5b435ce701ea5a84113&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

