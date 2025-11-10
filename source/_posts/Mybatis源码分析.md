---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XZVDR74%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQD9paO6cYG2y062vF1QpG%2BUnN7Tg%2B2Xx3X1IyBjyIOhAQIhAMQEa67uV0QCG7jQGQ%2BOmqazfDjIH9VWZ7zCSBDy5v7oKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxtvN6wqZm5XDm037sq3ANNsQHg3c%2FDaMcGroTMdj4LGegwDBi5w5rFP1pHXtdQVAyFXl6hCUrCwTLsIjDc9Ou62dHbznLj%2BYcop%2FvDZMZNtZfxrRWEDY%2Bi7HNlrZ1b8xdlS%2FxEGIfxZjpqjUUwji9OkIy3TCGfHNvVh0%2FsTWEgm6bxEvUE5Rf4YXv%2Bxwurd0c0Z6OaA%2F1ukCeKBCkc6eDYuHhwVQqLGaMlwEOXjtZQ618UZW4qzhQxphLXaIbtlV8tqrq9uo97QIoszs9PsNLoDrpEkMTjHH6ExBiBqVEZah5wnokax5iSItDLodpiDwHi4JII0IGOvz%2BWvaQf60ktcGRT6%2FwOLFSWsokGojYFjSS4b4movdqc3pB921lY8e1uRiV3AitENP0d6ptwE4j3i944DNnHhRdJK7tL3W2bIbOySAF7WljqGXrVf9ZLjALeDZpuwhjAHwHJ5ON8vqXwMbZmShceiGc9Nhw3UehjsWfuck6KVCCkuZ2sdFD6AfpLX98K%2BH86J0jbJ533wuJZbMkcsBS1oG7tUP1jfrqCFz%2Fqnu8XYzejZER3d7X1cFoK217IhRnuzn%2FqGDdcFEvvx%2Bv1aH%2BwvHQqnOeB7bDnbVqsJdWk5qgqWb53LObXltfDCf8InnlkjYsOrjDN78TIBjqkARqIOv869gbld1kCjWyslxMeoiClmsZJ6L4azjpiumVo2LX7ExOxbIQPzGq%2FjewBOHI0QpRWrTZgnG9ZrQ0IQvKIOmOchRyLHnZHRs05tEJt6IJEfJfxUWvJd7R1nciHmQ2IKEP%2FznOXwn%2Fftml%2BFf%2BC1gxTbKkIMugOCHOkJYEg4HqAyUU%2BX6lrkNO6%2FiHVh5w6SgPRo00AmBn2PKQaqBLC69R9&X-Amz-Signature=e65800836adfe1687876a9a8c55f725b532afe43485a49553db335304d35bd13&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

