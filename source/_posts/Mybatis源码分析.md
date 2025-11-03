---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LFTDYON%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T040037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDF%2BkHucLuOgC6FZE6Km2uYCnTK5H9TCVaQo8NRDJvRFgIgVx9jWPLyEqZcnBd%2B2aQe9lMSZm4l04CoNFIE%2B8JdyRYq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDN1mS3gLFyBFYPzEMyrcA3c0GcH8WPpeIkdTQ34%2F7M%2BSSaFpjSaf%2BoypPm7xkEN8ROxAqAUTsdhfqEtbsBcT4vImhJxrpfwJkKHvzTkEkqLIQfzXEqehEj00q42noKJnTOxjcPJ0hXtoKn67nbfjv75lofXT9UV7%2FwG17A2M%2F9oRAMfbj970IOfOGFsxFB0gfZOkwcFCat3tfZ7snQQ0a9DX9HfbubqHaJ1rdod6wvvZOvx7GHuhrOcoSFJA7N4M%2BULUiOFs0xhMlpDyPe%2FExGk%2FgJiUqYcjRiUP85wqdEFLWslZasCoutqAnYs3JpXfZ5QLa%2BDEHH6AAjIu%2FgnXc7SMyl5djMnWwE0eLThLS3k%2B9ncC18Ow%2FlvliepkHZz3wuD81xda7imG%2FhLjG3mlsj7TIM4Ba0yxiucF7GYkWpjM6D%2FNOW7negpsfh1BJhxGEVhlrXPtT3acoapqiMJwLL4ixTzDrxdcgQohAqaycbhBpl9CjS3tA%2B0HydNFzwGxnASiHCJKtKhuD3abFPSoNzA2EPff49aWqYhVpKiVHCx8rBJ2OdnUOmYD%2FFOKTBSsFjPPaneW3qVx3gyfpnS3C9Q6ZCMVBR3sBSg3a0LrWkAVuAikwLFbXk92To9gwDBum%2FDFRokqxQtUhU71MIPzn8gGOqUBL3J59GHtzD3RKHsoa8ZtUwKIBipiG19Jd22aXLcZE%2BHb0gJ8byHYQcDou%2BhmecCJdhfZoT4dgjvrheg4J135wq1lgqPZwTXHXIQZYrBZACOkybIo6QhVYZXEto5HX2povB89eTWTtvioOdXSG5yrOBRZsnxETw8inYCGPLBIeOT%2FIffMWdEAIxh1Zl1yxIygZ3Zgw%2FlcLmP52WH7J4bpsrf2Qh2P&X-Amz-Signature=a86d002048334e8fd5a07fd49dfa9075af94e7081d910bec3c7f08a82f5eab7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

