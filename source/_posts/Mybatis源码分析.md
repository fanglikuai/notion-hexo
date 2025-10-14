---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PZPHIXY%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T170119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcPCZ3oySTOP9ZTHAte%2BRsKhlszUi2pwUuv45v40R6SQIgR%2BNn2b%2FccTdR5GQUv9ELqPanwOYpvMvsbn8xjzBM4uIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDNXXTHvFTcsbiaxC3ircA612EMTD7JjiHgINAJH9SRnJPAKyxwABSSDhQ0oMr8t9C3NDM%2FzKJTgasj9ywpsiNEBefnFO9tkWdgwFp%2BqBJkBazaIGbo940iOeMueXNqwnQjL%2FHQ8B7nlQ0ch51D%2BMzSIuWWrnBnCXwAEWawner4DXjy52tdfDlDC5DqPxMyr9447wX5Z5QJb1%2Be%2BUraxmJTY%2Bw6rzHr1YAYhqe0HqjxxUyxeV%2FACQxCT%2B8cIuGLw8TvL47pKxeQt673y1%2BjcFlen5%2BJBLZl%2BRKKhGBGN1sI4K8gDvRr2y%2Fw1d5NlGvd43vjtHLmiTWcvGzRP2HSpl9GW5Yqx9p1nc5TJYPtwMIvNBzcmBOfC%2FREUirXlzM7T%2BJWyrgj%2Fh5xyxg3sBYcN03oFLfME8XPjPZBJGLPiI0VekG6I%2F4IJdsmyjLj9PBcA5OtoiAakB3ZIWtZnNqVCN2AIOIZ9a2j%2BvBrKEKBLXbkr03n7eW7KTD2A%2BEUxY3daR7W1vy%2BwDoYiuklQCbNQXzfyACZfUJYPSvrDmTJWoUbNwBxacc%2BGwq%2BLs%2BCRGtX54jebPx1vuppBZ%2FmeakVgBhMf5w%2F6MwcbHxk%2Bvt7UWnwBJ5ZLA2Ci22X1Lw8%2Bg7HhLA6%2BcUNr6v2JjhG%2FKMMr3uccGOqUBRs5K%2BEM%2F8mwhuRl5WhKmbDbz4VWO8KR4WKeuwnScsVTZMjDBAM6AHvxK4Aukv6oOdTvBYlm67Pw5hwa2nSL9OYz4BY64DCVmhItK11DBkyXvJtICxFj8%2FjuM%2FkQUg7HlFkPbg7NWlQu7HwCrjj7ar7lBfmW7%2BDicPwVGj2zs8o4aMujWFlQ3h0vEH4RBgi6ulGxmvy1Msf%2B%2FvNziq3pBsVxBkKnK&X-Amz-Signature=e62da15315d69c2936064318fc980ca383ee88d3bfd0bde0de987dfc5539b7c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

