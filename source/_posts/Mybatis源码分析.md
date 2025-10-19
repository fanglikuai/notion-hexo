---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666UTCQKQE%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIQClKUWkY6wx%2FmE6v7gy6%2B0zUBlRsH3Tnxl4dv6USKASaAIgQ9HyWyeLfSpf6MkLA5XxHcll4yFC7v6j8JnHXStTSuoqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBlFfC5dwzvvSCFF8CrcAzXFQ8F%2FMAlky0AUYOjUiuPPpL4shyyCtj875ux%2F6h6oFrJt91Z3ozc1hSNw7tj8y2%2B8Aev4Nm%2BMx2bqpF90CsdzCsq38yYeEVOPXL%2BxJY8zpzLqN1faWH2jULDlrK0ftoFVPmIUQ%2Br%2BMRHj6wBD7FTTpQfSdqsIdC9MgQNnL9Un%2BeiEQbUcJ27bzGCXr2y5%2FuUK5v%2Fx27tUD9pQTGtiN%2BU6ibgK8TsujqA237TQSBQ6jK%2B5ARLZAGgDfkR%2BzVVu1szhUHm5Fav2G3hvsggUlGsFBxNNcD%2BdNXOVHBPEbZjmcZnpzvHKGg32ti7IsVt4Z3YFk%2FsdyTf7zNw%2BUdLbWwBApoWpXqlsQ%2FDGY1EAq7qBD8SxeE7ON1UHVVFgM5LKj6%2Bds0aeAAeTs4L8jMwiXOkDG%2FMCuMIqqK%2FnstdK9GRkriXB79GUL5uMnXRZhlPmegNGIUtefz7kISreLExJYS%2BwUtN4BLKLVzHYbwjpNc%2Bw5Xwv%2BAkJAo%2FwZNeccA3c4KcjhBRdkuINiVr1zuYlXzARhKHv71ZBCQY2wlZ5%2BPeK9RNvlGD3hV2CSWA3DfgWTXq2JdwqS0VGkKULkjaetZjh4fzPc7kMTfR6uc8zQ%2BoCPrOs416IWS6WjLpxMNrt0McGOqUB1lovKZSWg6H9U5PqUY5HZV34pTA0ZwxCUUcHlfrFdWrIMyumZTO1dEHqfazrqZjU2YdjOiB%2BKT7STo7RV4dn19BTopOe5yN7YLDTEsBAizFBmNczsxqz7GqcB8D%2BvN6rXB%2FSPXAzgkqqH%2F454RyKUc7fb7%2BNyFRFK%2F7tufW0%2FooSzGQlLWjb5snV0TWq1ECUrvWJpopOw8wRUIAKNQUY%2BCrm0dKp&X-Amz-Signature=7733b242d81f7b0d683f9ec79e03c0785c5bf7d83f6e6acd3ce76a0d95cde358&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

