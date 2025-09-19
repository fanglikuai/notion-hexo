---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVGDGEWD%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T020148Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCICWOqKNj2tvf8tkrn8RpP3oxyqGEddsbdFqCjkgMTEgBAiA%2FPVw%2BFCX9ReERd88ACppK7lUI4slhvYOmDpmc2n9muyqIBAjI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1orxCkR8XnFoUmusKtwD7wI05W1b36lSuoIwlz3yD9vICVXsmtZFzSe0wPH4aQGg2bUnKwvCOKi%2Fl61z979%2FFak250X0ea1Njvml7TgCDhsmNZq4jSQnKchOU88K0icAem%2Fs7kZbIYkpdHJWFhLRdLtOUU5Qulc6SLY%2B3fl3KjG02zuTDwn2khRgH1KajiCTr%2BK4Q3uwe4%2FSutHn8VQcaI37UjVk4UXrCG9mrGKxDUue0hqK3LvfxwFSTxnBORwjHe%2B%2BZ%2BJpI3I7%2FrOJI1fmEcWAorlWl8dVygGJ6nlmdIxoNUY576LN0BeItC0o1stgdNOyO23XUFoYaYaHF%2BO85NnROrpzIMlUgM7%2FRd2qFdqnJX0uEhfWo%2BxLW32RBx%2Bwn%2F6rheGmgVP4MGLDX1jC3ysPapwUFrx3J8uSgCyP5fNkbkdxfP5kSQSBUqH0nM1wYQ%2BvgpO%2Fp3i8wq4YzvyN8PgWizQvtZ51sin%2FJ8b1y3c%2FslGRvHQp7WGGDI1g2LNp7UuoKIMKSpMYD19tlmadewQ5NvsEr%2FjyOYyHa%2FbwB7nmLW%2BLJTi5zXxV%2BTywDvyLgj%2BM3VDAF3c3xLdRvALlNWhNEshDUMQki3wJRFuR%2FdJH6N0IFyMgmAv%2F8998lb7CwyxUH7de7HTe0vIw8qSyxgY6pgG%2FcP4C5geOdrNMDQZaz7%2ByXrjXDe7purF%2FCar3kN00JfOrhIGK%2FDzfLEQL2nkmTwNYwKr9vBoCQXFfQneNkEW6FhSqqh7qYHmXd4PXGmuTT8MSjg3COcQ9FSHANirsfHRMV3kHQQJp%2FA0AREJ%2Fq9sviAcMjRFY40G558%2ByC5W7ekRcHlqIKjg4wHm%2BQfTWv9ZXA%2Ft4vqlkQBkXW%2F8PZcgUcx%2F6vjQz&X-Amz-Signature=bc9f91997b3e66da6134655a02428f6b63a3af8b6c4821975cdeb5573f902544&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

