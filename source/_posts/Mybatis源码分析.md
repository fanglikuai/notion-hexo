---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAC7DFQZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCB7paNpxcNjCLWZB37GehxsZrzDX65O62KVvTjd6NZZAIhAOXciaZVtZHEXyJ%2BzhqSwsRjTNls%2BXi8Ha%2BoQDyzPmyBKv8DCEAQABoMNjM3NDIzMTgzODA1IgwugsB1nDXvoIJhtWIq3AN%2FT%2BdFpahsXNx%2FZQrRJ%2BZzjilF%2FS6qipj%2BJhwlGu%2FmOe9y1bW64aBSn7zsj0CCtXdxDl8qSyTV8sO9AP%2FjUvwLyN71xXoGB7uUbMN8R7dsShFo%2F929XVmbgTcnzOdYH1cOoDIRtr6LXvqiPapG4Jmv%2FtJrw4jSFGIdpfbiZNd%2BmhoRIo7%2FiZw%2FOXSJT08N72FfyTH9n5R1hVYk906ky%2BW3ELRTwZJEo9UCifV6mF2D6TD2vGO%2BZrJ4GXHiPaPFppnDvXg6fZc153cTRXd8ZK69rpdwLCvdKqXCF26gBNjyRQpNXCcQJ58e47FHISpZxxM%2FDSAjJs7hGYrW5JtAQ8KnMw51BEY8Z1viuofMdnHDmBSmp6GG%2BMCZ5LhzPVqcygR%2FxpR6yT7rp4mvJGq3jv%2FAncrx9YGM0qi9gc1cI1Id8T15lJ7gTYg2jng6kiK0FxKaGtUJuG%2BLFnfhe8u45udD0AgwE356nFxpMYeyQeVlWlnzGjLm2zxC%2Bvjj1oQKUiRp8sL60%2FxkOO3Gwz%2BmIMoqEHp66wB2j7qxcVpX%2BxOJ93kHOIGBR%2BOYBumQSb8n8CSazSIhbzt2Zmzcgh8g%2BhaU1fCQhQxhPex9eWYOUIjkfry9N1yNX2OMjD%2FdXjCWu4zJBjqkAevUo6YkTHn%2Bd2uj3iO9A%2Fu%2FTKeicqV9g750mZ1kbx9fji%2F0vhp0LUIZhm45SuvvYrOrWiO%2BZKhtSEy1qo67glcaW9p7RebvFmewrJqCafr2JiqlrTyo7PkuYTAqCo3uxnaAlYp4GlXDiVENdwgCZIEoSCBFt72QQb3HFqQSj1XwkroPlYsmoekq%2BuPFkTBbwE3kweqQyV8HCZBxZCi7wYyKS6PF&X-Amz-Signature=8efa017c40b3af4cb1c087367550368d69d96d9a58d0911ceefdc59d8a24cb55&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

