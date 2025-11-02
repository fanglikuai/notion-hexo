---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWORSES4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIDgo91qejHIPa5Im7Zoyf7BprWEpkpiB7mQTUj4uqZ9PAiEA4rfsOBiu9ZE9V87M%2BHaturNs8NNOfHaDzTdokk91n9oq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJoPVsYEbAn%2F37RAhircA2tp%2BjndzzvpYEV4aakZvx5eVtpC7x1QGBhA9udcUdXyW8J833J0jQ2UqPzj2USl8H40z0wAqNpZwgl1sG1VhW270q9DBPIXvG7kEbmKAJTW4FOD6TJck4Vnd3lAEKmiF8sQMKmV8h5uh8zgZUYjx9vGyanxixx1MU4FiswMoApca5WCCd99D9o4Nd9sTZql79ubmBMdVZF32L%2BXxSoFV85uBOUjKbyJicMhgbIGnhvoEk6t9Vio9OLiccAYxuJVrx%2FNEIdB%2BzVopihxzJHdKV8rZIEqPntbyKVyFG2v4lcOIHCEw5jvoUPZrmpwMqNqWjcv93F1rS26nK3uYjmhwLMPtXI%2FqL6K3UdTGey8M3mbD73Zg2ROQBCNwQNm6P%2F26X4FwgMKR6obYKe6ZdmJ5wmczZiMPXaQ1aVC3uAUSVNK%2BgS0YhzWPDkDhK%2Bbjz1tcVHbBn2ToaRs30eGdZ0xqtAiYn2Fa67jSZmC2F3DWiBDEJ3SoMwQqeFLxKZZgbY0WSZWQ%2BkpGwaaOaNigmGJch%2Bwo5u%2FnXUl6%2Fz6gttD54bz6I6rOX7NGTFsQYziZJBlRxg%2BPka8h5NSMCUPg70B4wbQfFLKf%2BrQ6zXwCkYRrbdsc16EIX5k8NpkOZs%2FMOTOmsgGOqUBb3dpnUdgN7Gl7fZ58b1RJ5aCDapxJs5dBUpwRQFHN0w%2Bq%2FM5DKlNWEic2gpncz33k8THLCMjGs3rkxml%2FONu5wBxQwaPtbYtSCeduxJCJ8fWxk4f38hHbLWNrtC6q0To4bEpMoz6z5wONDdBl%2BJ9lneVTL5FkkeDk0fgY3eeMMG2cowqUYExxUgzVNvI3QkYmnPIS8H2T5NSEL2gFFe3ydy6plBx&X-Amz-Signature=ec3d2d55380bfca9aa77506ffcc3957383ab26561a4d6a865c70081cfb9a2e39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

