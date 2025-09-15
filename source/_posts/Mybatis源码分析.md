---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSV6MYGC%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T085604Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDHEwDi%2BJbHCY8FhpsvcwCfrKkQWVGoDWOJOh9nPRse6AiEAsrNQkx4YBKlz1LDmlCJGtFr27WjpqB%2FfgOJ6vOd66Nwq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDNlPp9SpLmXHHjoVSyrcAxdqTdVruFbsjRIED8CdHw%2FqTEDxJo7sutGDnEY2Yi3wwvqYelQlGcrOSHg1%2FNCzHgYOl%2FFF8TyazkMjoVaCm4xDXJIqgeicM3hxxw6LQh%2BQH%2F9Qx40Q7%2BVw%2Bs4qdOmrF%2BrKM3%2BI4HW09ROlW8kkve6oy4g4ud%2BDnG8tqxbeReBeICGo984n7hFKH4A3tJJZUFgF3kefh4UgZRMhK1ts%2BAfsHDQ5q6c%2FOPuSjhQf7ySNdb3J9jEL7BCJvGYmkwMvX0vedHZsUVqznk4snfMvhjOU24pAgo0PXZhLGNANgAFsD9VedFzjrU2e0thZFIYoZx4NSohoD%2BrQXVc17xmycd0t16yOCwjb9UQObp6pm15nd48cZq8sgJDsqfYMFXDam6K1bPNQBfvDo%2BWPaTMnsXN9Z4Cu4JoInYMQxM82eAo6SOo5G%2BAE4vu%2BmLFIasqUnf4pJDmvjaiPVvTIWbQfrbr1WQYB69YWtRgdWbmPwf79OClZW8AmP9050a7tpoM%2BF%2FwwM984Y130Sdi4etMuIm69g0QOFiPQvhXihPNnMST0gp0G%2Fl%2FMyGAKwxod8%2B5Yp0NCkzCkkvVzXgyk1%2FnEQ7jsYEtkpWohczyPXwZqn%2BzICmFxW4JsWpCh0rcvMLWTn8YGOqUBpdavTBxjNR%2FohKL6fgbsGGlHilllFXyA35BgP9ce7Jrip6SE69hEb5D1EfVkiJ0Lih8pATFR1X8Rd02XHN%2FKEs78usgh4nQQJIsLVQVECnmJTLqJDewtPAQ8193UFbQt06YIlxos0hPNGY01NkMyyByoQY3QiFGWoPjLKLA3oXGhncDQy6s962vKXnatXkiL7VN0nCPvDznkUF1xvETPAfZ43AV6&X-Amz-Signature=a0dcf7e5abd34919012e072730165e9e0c633e59f123e804305078a0696622bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {    public static void main(String[] args) throws IOException {        //1.读取配置文件        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");        //2.创建SqlSessionFactory工厂        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();        SqlSessionFactory factory = builder.build(in);        //3.使用工厂生产SqlSession对象        SqlSession session = factory.openSession();        //4.使用SqlSession创建Dao接口的代理对象        IUserDao userDao = session.getMapper(IUserDao.class);        //5.使用代理对象执行方法        List<User> users = userDao.findAll();        for (User user : users) {            System.out.println(user);        }        //6.释放资源        session.close();        in.close();    }}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

