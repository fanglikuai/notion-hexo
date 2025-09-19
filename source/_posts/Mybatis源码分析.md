---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UYZZBE2U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCIQDorLTKu8oMQeN3LDhfA79K5IugEN9gnG7Y7v4qEmnvhAIgNrcF06huSbHB%2FboIfMSs%2FN3zj%2BauapWfLh3b5Rh5IsAqiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOKplGqnq%2FT0oYsO3yrcA%2BrXnEDl5l%2BDXQLmwQrcwBNA0xQSuSIGWzGaAaWHiThCJOi1ypYzP7cQutJlCLe%2BqKLkDkG%2BaYpmojqGhEqBBIc2j19a4G4tkTC7nyiu8FT6q6jCYJdid5Ow5UawtxGwZUy%2BlHJ%2BwzTlOfczgvHyaEAZEUKMAOI3Zpx%2F1jVZ1ov5bV8a3UEMLh8xj1IdGqdc1Bmpug1qcA2X2e9HrZ5NybrogrWbXL8Ykz1hdGpFzPOAh8AGbJexukP%2BBQN58W4xajrLMrvQctwaqMDYL48p4vRBWqaMkK%2BTTbe3GvK9hvrDeBo44pnqddWD9ukLGe4ncmyPx6S1nOXntp2ZnmUWml3iLVYq5W66O8OD%2B5RucAOqmNY1THAHLdV0qrPiNzZXIxaaUHc3wZP8low3PRwT7cy7tAC2hc%2FZvZoIypfZYCl3pmkye5ecG1X836I9v%2BceRPy5YwrnQvL6sZ7MdrhfC70eX4x%2F3NzUXsxhj0Mx%2BTUdG%2FRo37FBu54xT%2F%2B5FdpnURNhtExGDJ9c5OsYykiU7lhKw1wXXFKfOiyTZ9MvxzCfehbKVJIvx4pkFDWDrUk8caojOVUZXPjYpRQ73r9KGOI09rMJCnDT9tTue9t0RTzkWtMvXOz5nz%2FsBls8MLWgssYGOqUBbzAcpgexknIerQs70q0qURoRqc%2F0Id8yMUSQFGbboIH38%2FylUwtj9uSJGiGpPF4ziv27cZIWBmGEJy75w7UQSV1fM44ca7W0QbbQSHHuZW200%2F%2Bb%2BKAXf7NPWwDV7y6LC24uxs8prA8Bc8J%2F5FDxXVlap6hhyEegUrQpJ%2FVmVZbhXzmp5Wwo1B4%2FibDDTngmOoWkxswOLXHbCsKm%2BYlVCFehRxa%2F&X-Amz-Signature=bee7705ba531b8d5ebd70c1cb4e91afd360b2272f9c39ee719bf7cf3354d1a03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

