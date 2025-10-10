---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4Z3PKOX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQDHCvXRL5Wayyue5cspdtu%2FlKeaF2hjKkFxSwJKzIYkhwIhAOx%2BKKzhEqOGwqhah2Vdp8mzRAB16cQn%2BXkO1SPJ%2FEhXKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgygL9mc6UjCezj9WZIq3AMFhzhF2McEBO20xJgwRqRgytMM0DrdJzvbaO2TJdxtlbXngIlajafonunhUbQ71pdu6zLRJY6vDItDHAl24aAdapqZ0mUZt1DHgiI6ZuE5N4ziOLhSZcGQAxjIwkfN9%2FYzTc2orqAemHR%2Fe7EGxOFySQrZ8lKjPHoWj13MOQYdPah%2Br0fUgMLdou%2FHotSwejCnYryLxT%2FKZVGRIUBcoq5jrLRC99F33vfCEw%2F4WEAJhXicMMWW4hjzRkFZUSyrdNJluqLf3630%2Bg3Mf8cm7kRqrzcxBEUgdCLPwack1lBKUwkEjAPQUdfVUeyl0yLo%2Fxs7Fn0zo1jISupwMdS4MB9l5aHJenMW5wNV%2BLPjcg9Ag%2FljkGekVBpm53bYeaR8F%2FigKP2fviEoZqQ89yV3cXuUbHTZ%2Bju0%2BELg4AQ88o%2BD7PokbKSMdpkuDk8aJLLhrOf4sukmQRM8sN05fsPyYwScH%2FMo3mRPJLZrG%2B8UHnGedYJVkg%2Bqv3D6nL2KBSaoxmkJqhchrJW5%2B8PEJRSJEqolBjkLu56cht3g%2F6ksT0rdeaIMNqbF%2BQNavFNFKl5jqrZrEDwLG07rHcsKg2Qow0gJDmy0J%2BsJAJjuxbeF9HKWD41OTmfOrkkOjMMEBjD1yKPHBjqkASL4xhHFyYrt2PDJ0m06x2ej79OFpundt4vH1DK1WA6Ddf1hYrhUQpAjOOt6D1t7AXrNgdJRgMxycYbdS%2FlKJr6CN4jjW%2BWYM7SJ%2F9Z1r9r%2BIQ3LKiR6i05HaC2Kku5%2B0cycYNJ%2B4dg5YBGXx3raOsVGm8B6faypWWwFuPN1w5NzxUISBn%2FWW9%2BgGjJU8rgm9SuMsyjns97%2BbqU4KbLZTbdwuFLY&X-Amz-Signature=f988dab10a2e94e6827731bc02c84b2a36822049518bc74b8578ee460c3614a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

