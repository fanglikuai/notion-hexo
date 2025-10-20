---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664FVWJGWR%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQDDcOQBoFiRN5v1pMaNDLyyHuniFBO4CmEkwVSYBeApZAIgITNcKedurbhpIXddOLcJtYTscQNKrHzMt0A6WveeixAqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG8DfX%2FJJuE%2F0f7ehircAz8DvwIxCFI53qlFJHbcU70m0BbcxwzR4WYS%2FCiu%2FA2j1neaEoJzKBJ9de%2Fi%2FbaxSOd4SIf51bIjYFlK1Bn9AOHAfV%2B7bekWu6rzzsDdcXdpGJ2vbEYOLTkcbYEAR4lS4zfxq6zsazM6oFa5j1eCNyDeCV6%2B0GT7A1%2B0MkO%2FoYbFTV8cRr9I%2F87UxlTJ0f%2B6uPP61Of1SvFrQGXtgvY6Qg%2FuW5r93OV0aBvOXizqCWPhDccfG%2BwFJ%2F%2FeQou6mCUcWK7TwKLINgDvyeEVn%2BFM2R9ZzOHPEQunI36tp5cV2Wd9Vl%2F1nLluxmj2HuviIuF%2BekJqCDEcCJY80lpNy%2BxVCus9XfNhF2NM0iLAWz%2BaTXeZTjY%2FTvThNXnl%2FRYTCzOoXrYv8N2Kr%2Bn9pm1idAiSJtYh4z%2FG%2BICKBZ7oyFDGHl0FRHpxF8XsvaQf2tO7AAC%2BwxjvnKxLrgAbr1Q9%2Fs2ED6Nqy6JQATKk4lQTLvB6XATGNRbpcSZEQa%2FOpk1EKK6l4AYP4bKrBsK2zfj9pHbxdfg7yq9Ks1I0kg4UhpHXs78uMuTKENcrcoAyR3ag1e7Gz%2BEywuznWsUOXqVMjUkfCZ3vIXFoSus0DMAa5O3HnuE6XX4FuewgBR9y0wh0MPm218cGOqUBrKV84JMlpqW1Nwu3gY3m5XMarOPDAUO9ril1d7aGiZwyVSzDEJ3ZPC%2BrfP4SsJPOw2%2FgyLZfeh3yE18Xl2OuQYfaogipW7lPtsO4qWqRPeAoiisyrQsJkm8nrnwmVbIFKbtbxGQzdWjmBxjkN3%2BiIvxHwGqcDDOW2nMSaNZ4VS5Xvh8VYDg%2FzURuJr9HqhvDTfpZZzpyBfYN7TPbJEQMj3uDApla&X-Amz-Signature=a434349357c6fad26703a919261bf7dc9c2ae06aca3bb5acd03a878b7ad0d2c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

