---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3ZYXYEZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA5RioBok13lnpoiWqRcVxVM0ZmZ7%2BfCQuVF2cvukdq5AiBdhmJPKwLmuaKmGQfZ67yRRjyUY597lzZD17fuM5gz%2Fir%2FAwhjEAAaDDYzNzQyMzE4MzgwNSIMBFpe4mrfDUojPwQGKtwDO0UJtfvEagtvZFJBIDtSvy5jH3qex%2FVX5snpmEkxAlwCbCrI6hQpXfX5%2BZ4owtDcFnMLQlXLp4VcHCs%2BfzmDrVZIyq0T%2BTUe3eUX4cgqEJdFLpVprV2QyhPrjoF7S8mfLp%2F%2Bzre2NPx7AoiKfLLOTf%2BpPw58l9W9MmGPB4dxzZDMqWYWxlQ6bU0q1f%2B3xxw7HRLaXT2ghllCaBuP7zcbhxv6JO5USpZgU%2Fw7KJ3Um7aDLyKWuZjsn%2FlAUbvLohIm5Yen2AZC8nsV%2FclMDrIM2PMMXgGA7ssqT5nGsKtg4LTroe4HXcfZglRIFPlVqZ2EQEcWDqKvwbnOz73D9%2FGgiYar21StazGVZzH8%2BlrVAqtS2FOXC0T2zZBw0kqJOQX8mXJXl8mFUjtdP0Tcmd1dNRs2ochgMx9CI1Z%2FRqbQrcgiDT25OLP2RFkmhKDM18av0UbpGNehtzaoumoiZmbuVenPus%2F50wJ1FWkdC%2B1YxQr5wsPLm4zfvX4BJ%2BPq7vIdHfAaj7vy44IdeliG6JI0u335iEwWqEuFCcZjV29AcGRweXZUzLIAOPcoLIIBJBE73vRNlT%2Fxzs02GmA58I%2BeU5Q3D%2F4R9HnBN1kNH9%2BN25m6lGZsGi8b6xNNPgYws5i6xwY6pgF%2Fv7Abx8Ps7a4bXTdNfA0RT%2B0Ud0zHOwqDzfxCc%2FqKnvrISn2DdwWTLZbDC0bxese4bCjfKkF7hN0bWwiaUAoe5JEYvJVXqw3GEUyXbpFtcx%2FYnSqAxJ7oYqprUMCWhulSVe95qZvASFe7LNRPCusUuWpkAUWKEQSHorXGBAbuaHpa0jD0M1YIbVjCIexh2GlOHKEuOQs%2F6SGqj7zmXH3MJji1QGQ6&X-Amz-Signature=97be0337f269dc592b0558a4387f20404c13dedf7a6dc70b8f4380d52a33269c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

