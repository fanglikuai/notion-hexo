---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJ7N2MGI%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCrk2BgMyesf9v5U4b7vb26b7%2FpCGS9WJdlO%2B5IuMuXQgIgGksOMbkInpt4sCy7nMYrd63GvvUIHJbWmBYWcmdlgJIqiAQInv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5DKEh67xzkxo%2BdESrcA3LQAChjL1Gc63wZxZyc5HOdyTun0Yt8NGeKQgiAqn%2BEEMgB5kgSnhpiZpcPuybFWdPKtmnLiGPAVUZNcdL3aAhcoEB64O2ApE9w%2FQfuacKX1xPLhmWX0t4S4OyB7e1A0CWq8uk1lqvi1yJi2LNz0iFxCT0tp%2FPcQTZgNTCKwEveX8TGVTD3B33KO1XERjkR%2F8pmwT6iaIlLIYGsat0x1yPMFarCHbd5XZVnCATMNpBY%2BeX5DmT38Arvzjx%2B185oBzInZGSqLIny94xkyNOz2aXPc%2B0%2BRgoG6wp2OhBEfE4mYzcpxQJHrK2TEncbJGt1bn%2BKvWegR33y4r1FGf4oiDhDh9QOwZHbr19WhkhCKjh9dT2fjJQojODdhDgcRWWVJRJ4qL5f7GAn9IL5pOcon%2FglKzybWkFi6eH%2BuGn0WjeAXa597cy0ULprvWKMBNg%2FeRsIdhMktFKurvWQ3MsuLMyPbn5w38%2BYIDZ8u2TyY4Whhh9jjRtYWTi6LwO32DLL7RvLBa6B67Czb3vkYwM496fMTFfHrYYiBDx%2FRh87oMa3en%2FBPp019dJvQk1LUQmf1kq0ZACEst0QlxGW4QGEHugRVEvwAPXLPvSbcpE8BklBr28tR83IP9pvWEFWMJ3y%2B8cGOqUBzDpXkp1Al5shKEsUgwlj1z67j8e%2B1lA6uxeDioeT97PfA4GbO6QH4dqmSoq5O6LaCVm2spiU%2BmXdKhN8sf8c2QbwGQrmbggbocMQLbJO2J7Wp8vF12nLNgUjTdEVeZPLdX%2F4VwLcr7%2B0EzyNRRBTlg5C2y2be%2F3FTSRFa%2FmYNQYBkwmFQwf9xh%2BCfbyncOikzXzAoKxRKLM76eF8dJiBajKeSQjQ&X-Amz-Signature=e84c7eee5a9246bb6d81388308332c0aa3affb3a086d5aa871bc5ae9c32dd4c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

