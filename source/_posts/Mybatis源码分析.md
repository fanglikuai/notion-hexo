---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMB2KVPI%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEbKmNBSzT5v%2BRdaOPVe3M9IAhW6GHEID0ve8ZCtr%2BjQAiEAnVWR1GgaiLLOIr17YAL2CdhkDHf0jlRurZkSmd6Pg%2Foq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDB%2FWr5hoMeKkfiYY9SrcAxUv8FOC9A1arl699K%2BFRlmoqjOYvbjenW1bCRGM7pudf3cfNouvxgiH2u7ExyHZFCxm%2BPyeXjMxiKq56hddog2lL8URlGorWrH4hGOtU7OAYXYIXSAb6np6f7phI%2FrW0KZ4FhDUn07xcxtkPL7YwEJHVArPT5ilcmwBDYj6Xw34BiNf%2BwsLeQDxMP9HMAkLQpfM8lLPyluJNJuNjByweCcNEWVF4fIjnXK6EzMhZwJTjUeIk6z1BMz8sjpKzCQYaCKwwnixRlhCY7No6Lt98%2Frk86IXOxGNbNofFVuNybJnejUChTZgsHUPQtfEr76PvHSWs%2FpKFSLPgrop68%2FdUpzFd4jydC%2F1oEoX4RLQVloHmpXgy3yJ4SjJNxTfqr0PDHCgJyWhF1tRWMsvwpxg9awtYCDd6htorA2K1dQYNPl42ohEOMPLYDqYcgcS2tLcbllxiTwXgeLjSW0j10D8ysmbnf3JgDCXhzTLTmPfEWsUMdDheXAAg%2B24JgW3sojXJ8NNY4HPZsq8OPVNuXYr3WdaW9PIhV5vebr6Jtzl%2BSXBApc2Tb46DHyn40Bi99rEBWBm%2FrZaMaU0E%2Bk70nEqpXjiX3Lt%2FV7tdBi7dtAGVJAsy68ecu8nKwWshp8xMNmmmMkGOqUBhymAmlaKbGbUJx1E1zhGMakDGorvAOjImZb3f7Hu9bGbfBkdYCSo%2FF0PnuoH%2B%2BOkBmAnw8OprZomUYzZxSY8fTT2nw3HwLIgEadPVCJ3P1FhKUkaPj5%2BG8%2FqlIVPI4jD%2FvwEPD33OCSkPf7c5jaWQNkZJgg7twQU1yIg9n7anWTZyG%2FF%2FBXxwxc5xsjN0UstaDlA07HEtUcOiqJTPFZiaIjPrP71&X-Amz-Signature=a81b7bd157cf76804a2eb2490bbd32cbab864ee8741981a4f2a1577fbb6f0120&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

