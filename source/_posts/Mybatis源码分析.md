---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOLJQ2L%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCID%2BuNytqZkqKjR9WDxU4tyvxy5WfYD0s5ZD3Kj4hFrlKAiEAy2zKAYe46DkIgdK3poT6cwxlBg%2Fxweoi9jy7blKN4SIqiAQI3P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOdj5PbGqoRjJtn6fSrcA0I%2BeVSxCZLGbOkzcr0ec5IClVOoWrwSpqKNw%2BLcMkLw%2B7D3TQL47qzOl8ZMGm3DT1xtjytBBEPBhoInpCIF7IXUudgyT8deChw8xYlN%2BH9anDZXsD6DCpg0Drwc2lghnUtOmMJsN3HHPq5cM4bnK9kQYmvVhcN0VREdS5VMMxDX2foz72BNaGCMhu1bpOBQ6yUvqCh2eqeLCk6R9BOer1HMSui9jOj8zRwBvALbBb10YSCgWy%2FdCfznnPNQlCYQrxAUOVDaVWYMk3%2F9jbMBLv5q%2FAO%2ByQgRaeW8gcvTYPtBB5goUKySTnHbuBf4JbNrgux%2BVKDCYZSsSUQqP6RAASxr%2BMxpD9OzpXnvy0aMoLkxt4QRGGcyYMavpIpiSCTVFVRVfMzRd6LLi53%2Fsi1BW%2FHvym%2FiGA9spILqE1evTOvWTz6wb6Osz9e5USwohfrzdV1LTNjHPwBV4U2NUwDStVPLnAsABOelZzBoxird3YIUwh5dq0y3Fld7oQCHgt5TrMJhTpVH89HrAQYnFKGn5IUBLcPKKDgC9b2NGVRN7JHE0NuCzyV5EnbhSZz6vaEfk%2F0sc4QYIw7SI4lGk%2FQmbd6xHQU%2FzMn00u7Xg9G%2Bda%2BQl4Hu6mZ7ZiKXOk5TMJGavsgGOqUBxbtGNiAkqYI9MSCBPvzm0MsJCQIziFhXxUkTfEiuH%2BpY5NE%2FYHCwMhhVKYsgo8sse%2FOmk53gMn8rfVzdP5gUI0zQcmkjWOUhUGf72lVF0kxD7S1svbXjJsys9mz682VFjgjFvYJgNbvdbaS5IjrjUBBgaDEIMd1gaAZwaN0vHpaNkbybRFGpATSo%2Bhqjlxrw2%2BoC949MYE5prcxhQjTklGFWvUKO&X-Amz-Signature=20e79aebb49cce2006cd1a00a3cfa390358be91e5550f84e5513f9554d2bbb24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

