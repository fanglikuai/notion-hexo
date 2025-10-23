---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664STOKOJ3%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCIA9%2FJ3kybBg5c9BaHCggq1BoZRPtegiOyS6PTxNq6jgIhAOzUSVsOAmDTzVT84rE5NMQEa2cNS6XQFUEe%2BgwdpVbgKv8DCEMQABoMNjM3NDIzMTgzODA1IgxMYypFhylZ5waU%2Fb4q3AMHQ%2B4%2F%2FKRJqUrqWrySy8wPugg9kyyx5C0L%2BCFM9UNgj3RPM2TrqqN29Ge8Qe9lCGvM9caMzoJC4jKiyEFKPr2Hpq2f7ENCilh1pzu%2FrpgznJ7YRz3P0ZfWuf2ho5zu74oD1YYEnfDZGFut2NtiEjRn5iZvdc0sVd2qMP8jjr37ODTqJq4TyY2Jt6mcEnk4gcJAl%2B9norKwOBiFRXv4dKzrkZvx9aMD4E3%2BZDwnKvm1LtxZCF%2Foy3HuU%2FstIjCblFVfkBT7xw6DCTjmKAsUJpzHr94MqqwPQqiOJqeJ8cX0fdJyj7OfqRRKGRlrPdiaQvwDg36tuVMrZ4sXSDSwbXUBWyxa%2FUFQLSbvYGhQVs3voIo6ei1wqwuWl9BNwgTA%2F6HXRTTgqeXlnvQO7yvnSudfYZeFsq5s6iFI5YKo5yjiYy%2Bmbpx4KNDgLxrTO4IYnLzS6ZO6%2FZl8gIP5eNlZkmPR5%2BmBLXPDq3afb2SxqBSVF2nc8PdW9PyCL%2Bd5yKP0txXtA6C%2BVVnTE5yEPOi1FtttbH0tKv1GmIT7gJSqv08w9pjvgDvSJdyR%2BJ8T7t0X%2F0Vk7eZD%2FhDTdNaKUyHOkM8Efd0MvFSjqnSR1KYPRjUatJPQsjYAWHj7BQkUkzCX9ufHBjqkAVBOi0mnG9TsMcxjKfdqELUfU6OGLq%2BZmdD1L3%2B9SwPgZw01BHWlm4M9Go7c9h3iZN%2FvJ1fpK794DPnofiIukxx%2FKlUhVzMddqhCRAr%2F%2FMF7dshaupL9X6zGi%2BOvaK8Hv9ma5%2FI5Gf6%2Bo%2FTXuQ8Baj5TI8Lq7t2k6y%2FyqoHlsh8T0cpkGIQOGO9LyZdtzIL02DkbEnWTV3yWqKzgriRh%2BaUYAIns&X-Amz-Signature=ed1c6a91f8e69b78aac0698941eb1ba7b89213d174d27fbeb2590cfa4fed2575&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

