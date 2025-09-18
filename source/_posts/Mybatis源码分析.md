---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HENSDD%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIHLooJL39329dx3oYq8GvPUxVhOANeQOtwiMg2%2FKOlUVAiEA8Ozt2XS3ilnGOJA9iaPYoyh7OpQArZMXC%2FgZihQhCSgqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIj9zeSniFdUMPkGsyrcA%2B0EwhIQU3yejOGnZp6ce23UgG397EWwDk8fmFWL7BsjbahZhPwjtrI5VlqqNhLcerInd90S47Z76Ld5s6Rrf0UkUBWu4W4jgxy4ftoLimgEz%2FQRelteifOLFZVxfcu8wEJOmGCaMqqsVZU9Xo2z9wC8DObiO6q%2Fyh64q%2FiC41ZRnAiK0gBjo38JbcL71Hll245QhFklduYEcqVpNy9xECfoN%2BP0lgbVE57nXy8xduGLguYDCA%2Bb0pGs93EERxW3kgmcGJs78SyX7piswNzLb7fkeSWwHy0OHpWPZI62rjDVFmmm7c0%2BoCWvgR0ZOH1wSwUQWLL%2BK8h1c5XX4n5lRyxe9mNSSTQ72SkB%2FNo9b5JzAiyGMKvfedTb%2BVDYUu09mbOLqqQ%2B2uBC354DgI1vT%2F%2BH1mtqh4EchCVV326eLhz%2FX%2BA87ju1xCZgA9U1bQ2CBU2BueYv9dvrm9lJDDV%2FUhc50GyNlpAo7YqW3WyX2BWj%2Bterl278C0%2FTKzeDKQOCYWsRdWK572oIIomIzQLALzJ73jNSnnOWdeJp9nFwXe9NaK3mv7htta3SbjZXCU93dTnERkI%2F010KFA%2BK2tCPEsny2LT7YEzS6T3ENVF%2B1Gk0ow%2BwS%2FMBbKFZvgVyML35sMYGOqUB6v%2Fck2dX4xpa2aGQugnqrjtyry0qkXzds8jM6WzEtuTyZsgAp54o9sfIQnCpzrO28unf%2FrBZL1l1%2Ft1PzJvSnQXtf%2BdYSc8Uqva2WJdyvCcPpfyNE4zyYdST77VEmCLGYCu6uNvvACSR8BfVI8OcUZsfd6bxJc6fku1Oz2oS9aqXXF0hr0qAQ7AxNiQTHD2KNIrx03o%2BlhhNSMHHs52l1SJLQTtS&X-Amz-Signature=21ea15314a3ae38f929062fe33f659d3c478d060498d99cdf7c904af0ce21959&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

