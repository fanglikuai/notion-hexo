---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGQUK3M4%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQCqkQfxjMBaB%2B%2F4CZvp%2FvzxNHuGXajc%2BMg9oJE7Xub60gIhAKPG2H0s7bG%2FgxTUHogeT%2FzJl0RNf0aBOljaem7dtqBkKv8DCBQQABoMNjM3NDIzMTgzODA1IgxhO9UhfDvEpG1VoNsq3AP%2BLaVvfKhQtgbeNxVsvyw4CmXDriDtEygYAKAydG7yhXKO8AbEuE0xnFRUSzhgQQH2lvb6fwkTEFdvSgMiEv84YUSkqs0DYceybYtz0fAuDWN%2F9msOPA585bTbDmHcbAKN69DPdG81baMpEDQRHyBN4%2BnBaxDZ3XEr9xlwBo4wp8XURUS0Vo%2F%2BLa2HfloKaC6PioxnxMVjf79otLrpulPVun1xZj6xPb8yDX60SoMGRyEBs9Py7yy4P9ZzCxDrxIWdTJWj9WbrTviamOnR52sfpjEVCl916ZwoVxjJIG23bju%2BNacXbj5SwbVlKkNaiK2MiwN9lzTC20UL0HZMib3rwiV2wK2CzXZvJuUGbtBboGeNpIItat2jn9aO2o1cjmSwGRdlZG9DuYABGcdKbkQStrr3axxHUVrrbiXlbo6itgolSFNYR58MNO4GTUKBz6F1HmHOF1oLf7HvKOpIiIHxDllsmg7pwo7AIOmVOkaqwMAnBt6LWg3QRv2RDU2TWW6785Y8EwdRO1UwvLCB69zkzQp3JfcQNKYcBAwFvmf5RFzh1oS02b0N67WuUzQzC67mW9oLkot1ahMpjt1g3TpEGTa%2BFm9rCp2WWJpWhh%2B7%2FhQ8tnxs93EewzQbOjCh6YLJBjqkAS9FeRiQbKBS9MDpmgQk96LZx9aH44NCaJN3AxfQwJhpyGLi6Z9fr0XagVD34HA71FvltZJysrqik0I1FkCM2okvPxdG3e2EIo6%2BFsNmBTdtEEDX%2FCx5H%2BQCFl7FiFccUeAQSAvkby%2FyE8etuxjJmluTZ7TPmC4ZoW6Bd%2BMZNs110KY3Y5hZ5WmtbrqEkWb1DUHlwcZQL1QHlkY%2FGEiGW%2BR1M3Bm&X-Amz-Signature=8694553db8542e4b49001eaa44e5d95f444a5e0339ef1a281e9c6b5e0d789f77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

