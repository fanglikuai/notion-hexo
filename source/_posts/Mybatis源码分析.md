---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JX4ATI4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCMD30GMCwfG37JqxVlUhsBF3jjMIvJxfh2WiGcozPphwIgJ1hGzxTVIApZy6Tnb%2BrUFGoyLGzvOzd4ILIDXEYLRKAqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD2yn3Li9KDSZvhMpyrcA42hK6H72MqzQSN1npIONkVXXydflfUxDLINe3me7nXV8Bn%2FU4e%2BrC3yfoc3em%2B9M5uK%2BJV%2FIiHYXLA8CURBO0aSg%2FyRxGQ7ICHZkE6Hk4aHOItdFzy2uY%2Fc7wr4MmKq118fvZ3i40F%2Fo8MPkFT4vbpnaiIiuceJJuRh4hXYiwdxO6J2SLuC6UHE0%2F3kN%2BqrCznkSq3V5br6mS8r%2F5YRtV8oLe%2FUIZDxrSgMG4toD3eh5OZgT%2FNWtK3m6B4r9ZLSJ9pFKH6qduFX7oG%2BANNVDJ32JuzIoyb8kKu3nhN2zWQ6nw2mbwoUb1ItOGyPDtQW5%2FKRjHazdRr0bG6SoB%2BxSg6Of3fAYS3rD3YIBZqnUW5VjMICwbCjM9vryA80rziTqoSu0vUlMPk5fi6Tj7ueADOcZJ1S6TM9lqdUBO%2BWuuQb1bgG7SHMe29p9OzEJhU5Gv4xK18seS02kC1Hj%2B8y6EU6heLjFjCoTM7QNoW7erKHjRBSAX3wNoYW9L%2BQMF96qO5wZcUHIOevt8XS5s94h5ynb07Q8W5FCfVtHJ0HrASid%2BCX%2F4ba99Q87%2FucPdy9McqFsZl%2BqCtX3iwUQyN87A6Em1azu7BpO6DCmpBsL7OtKWangyAlR7xBBwKaMJjj7cgGOqUBjZVRwY40i5Cs%2FyfRB8jT8f2a2HAPIV5nW1iRjlD%2F5FhzV%2F85Tf%2Fzkkf6lG32tuZDo0fC3eBy78mIKTl1riXNqdUKTUP9PFj6Y3dU8VTTe%2FOdtXTpomKm1eWa9Z8q6z2gufeWcauZaA%2FTmn16vBUFptrfAY04TtLWOm5uf9%2FTnN7yOXxhRda%2Ff8PLBuPbn1CYv3c9t%2FPexNEWqwVrl27zIjV0B4XV&X-Amz-Signature=aa463bbbb1e391f708069855e7b6f0321ccede6f72bfe7c67c663594ed4645db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

