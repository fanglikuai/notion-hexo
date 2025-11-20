---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2GJBGM6%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIQCwdZSUq5QsHSya9TPfCaOFofaL7fHr5bmPoE3D1qkaVwIgMLLVrJRyBUV1GPRcl6631NGiaTVITNRJp8kEw4xSa4EqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLf6A0STaMO8DCp7qircA2l%2FRTFnev5OrpoYuOfqzILwNS3j5pPPZhIhHgZKnH0yuPC9ue8kz%2FbjvpJf8PAmk%2FXfTOi08Vg2cWllvbRfHLi0rL4YQmGhXYQgJIcdewnvO%2F4R1kKfckF2%2BXvY90fXXLiHdBLfh24L0XPzSzvIWZEj7c8DaEB%2FKQX%2BfsCAV6rfAJjztDuxdVFPOS2fUE7u3nxlqPk1I4cX0xxr2zBPThbynXfsCZ2WW%2Fsy9eEfHWpSZTGtLJORAgYflNzYDcPCegNom7tX2xXbsRsCpJBCnJAkswkvUmO%2BGS48IAfy7n7HTw0EQxhS1NwasXvx%2Bo9tyC9KzU5k4JuwgnjowNEmQ0S%2F3khzRbybUl8PJbgZiNqvzM2NSm%2FGJLtASuhSyNrIehrwAraGwv16TVcvzg6Tc39DTl74VbQy1QmpBwKkI9O9itQwap2hWDxdbwmMIU%2FL5wvxhr9Zt0VvQ9EHqgHdOkwaH%2BZBuW1c6LZ9sJHJCS435SOeH1aHj7uoXP7Li9%2BCgmKhCNOyqEaMZm5fwXPVlR2P8NgC7UkauzWWsTtHKYbzhmjUwMN0qsX80Eg0fGMSgY%2BnBwGazQUAqlq2BHKjwCSFzH5WFVQlay9YIDU7dNAmNflxGRY7UAQ9ybmhMLSm%2FcgGOqUBuFRneosnDYfnusw9SCFchhXqHCLHiFOzsN7tbg0cFV%2FYd2dQAvH9o5r%2FK5x2dfTmFCoHRorXs6SLpvBHQi7hpSXAZsoZu45L9LpFyoC05r3My7htR1OQgkdIKIMaNFbeDw5cnmST%2F7YB9%2BC7fLleyHZUm0JTO84Q4BFnsibK%2FL5GUd6xNPRrJJOc9fleEoG67ZMdI4GeGXrkVuza2YxgGvx95t83&X-Amz-Signature=7ccbd9c61d7cdaa93053f3b73d0209d2d546408bf5d501f30efb900df03fe241&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

