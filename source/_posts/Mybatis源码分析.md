---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKG5TXZA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDr2a624LuCI6mpxruWwqi3Z%2F04VVIAwzbabwg%2Frjx4BQIgDunuG8zESg2gT9x4nW3jU6WITO4LLDXF4CmSmt4q3Zsq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDL9N16zzKkk186ayFSrcAxF5iSSoALQhija48FImru%2FlD7AH89i6B%2FZfL5hS20RzT5BPOxUs9NcHzniFh5PBmbYZNuZ0bUvqELid80DSknBcFtJ%2BgSjXf%2Frf%2F%2FngyTNe0T%2Fcw7u%2BLhHilW%2FUHcdiSOtlsXXqZdj5ZuRBlKticCRZ6WjxQEMrWxPWMWT5614MIF44ngDOexPFXpgm8lPFp1tPB2jjKJkXZ5IAytupyR0hPc%2BTsZgWGLTh6TnhuA2haLgHctkuMjx8PocTbe6lQs%2BunR0MNMXk7koMAkpx8WAzQwUIlk%2FPC9iFmBRm3kdWY9Bvb3wT85oGt%2BP8xbFA4lkFGfQk56QZlzqP3RDQ4O3C9BnF8NW0OyKb%2FOdfPHqTnnx9TI%2BNzEO2QpWEFgjyAlxllUCm01288JMkG2oNWCjKgrEsUDlmdrqfPCxVZZkiKeqCq8frHUgVPRMO6KAAbXna6Bt8FOd2W2GmlnwlnP6lbDuKHtK0ZeQ45zS7l7Tz%2F3SoPDc2PjYDglzp5G9E20RAHrrh8geKSeWmv9BfXIdi6smHVEBQTjWsjqRudey1KoNs53in7Sfjziu38%2BDQ8lhQ6TCvGPaoxLbhec4mCkmTyPtDIt0NdGcjjZycGrdLLWpuSFdn%2FSYbAINPMIeEhccGOqUBlZ%2Btiiwq%2BggRhlCxz%2BOgbvPZOf3tdgheDZswsJNON0HhEzkTpblxC9RNsSxLKn4PGKm5iGTvBapXio%2Fs1ooCHdo1BfVKNxFq4rv8tAZqepR699PAZXFp13lC8nt%2B58HLUGqu3GQQgzRq1QXoIX795DTUWGhcQLY%2FISxFYKAFAVWByLGGCdK4UAvcZf01u%2BvTkwXttGSqHWtb0EOxcD8Q42Zoy6Kx&X-Amz-Signature=2e45ca8dc171b6630eff0268c90a63e62e58d6a7d83d26c327f5debf52396a1f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

