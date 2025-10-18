---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WA5YRE5A%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIDOtNVtRE5MsqNjr0zYFZUqAahyPvY2fszxZxLd4YVC2AiEAovJArzN3BvtIizu1tVUHdARcGun5Jpe1QKzTx2p1%2BmcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFtOHz%2BkVjjfFZrjZircA%2FEqEgatkJZnYGZ%2FT7rOtKD%2F578M%2F1%2FQVULUC26gP1fUciBHuV3RL39%2F4diiFOewpv4FfIU%2F9%2B3TTVrFgUJY%2B%2BFhjoLF8hDdCsLvgPJzzGKtkdNjaMHS53hY8P4z5TGk8Fm1yt8AGyZQ6ebqX55I9r9FvMzY2NbRqSpAEGS8JgZHub8FOy9AWRNOCZd05BGcELXDNlMz7EjLReZ4esiUxhwXI%2Bx4c5EcrCL577OALCxNUMCsI1AWPCEltYbUxo09d9J%2BC0bkUSOnuMgWMKUKLzcecX47qo6cWIfkV32z%2BZpA93K4yHtuLWeJDhjKqGZVPGeDOC1n7J%2BgtpkhmSI6qlvq76q85EPc7ny4R8ePigp0aR6GESVWxUeLzhtEjP6UpmMj%2BaUCKn8q%2F%2FoVO73Cgu3%2FtqTMTP9SoUitADkGCjUy7HPRaNl7cqNxbvEuC8JZB0NDPaKMqA61%2BLEBfUXdO3Su3mNev1PLlTI9z6jM0MvR9QPhEeOymQgfAWPU3ZQa49eUahxHn9Q%2FqB68fmqBTgWP0LEHj%2FHMbbKELL0McU2f3qIJ8NS8aGYSQF3sqMHZW8P00qMHWSRp%2B0HcUfROwseSQ%2Fooi9f26jujpKuSgFugkcGsnSTxlrqS0IKeMMrlzMcGOqUBC%2F%2FlDAq1DezLmMfljhD4DE82lAp0HHykHqwFly1ZBI3Bwpx1ElO2UAzykd02%2FxsDcd9xrM0dKMlXxq%2Benf4ma1Pmn7xToicBlhyUCCUuV%2BWV9lv6KL0uclp9bSJQOtIDg2tMedNUwuw3RgDnmiD3GYfT4Y3t04yswzuw%2Bc9dk9v%2BOnq9Wn0IC037TA2967rAvh2O%2BH9Dt7LuvjOe4St76WBVhZMA&X-Amz-Signature=1cf012658c6a191b4354e484a24d1680b8cb8bb25a2b66bba790a59f70a221d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

