---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRJUL3A7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIDx0q7OyBUXLk49VtYPix5m4JvFlXeUGzIpnhoObIlkyAiEA2i%2B4TWPqi%2FD4KZuLY3zzJSEHikZ7RVcRx4wO%2FdawbWsq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDLCLJ0Evh%2Bzu%2FCLLhSrcA%2FQ%2FBvlDxsfBQlD92hjgthKXF%2F%2Bun3W%2FltiAclFrFqxPL%2BjFGskyjUiuJmJWEttERjz860xMc0aDJzNA8Cmt52sH%2BwYzOzfXUMebyv5zC3HBs3EBoIUJmXINbGDJsN%2BlVOrSrg9ExM7DaHhOk7Rf1pCMYhYW68ALh4ds0sdsyqoIAJnsfLeo%2FULtlo5r1qxTyn%2FNXi5Na8YUeqkefFt6wolRBEnWOMFt9q1ZWst9mGSWpJf%2FtJyQlL7YT9fb6c8QAhptzWaeVZXE3Bytr0DmDsl7d10LXqVDTHLVSXL4A5JigZnyQPZU24QfHvLkm9O7n0kl5giGfFhpwMSfSDCTN3rv5Em%2BLmdcw3tog18T6etgXa8rJuGmWphSDj2ZZo0SX9HxfmXA7DZYrhFNRZ0ptrvYIny1BeG8PHOLjyTetb6x1voakqpmutQhim9%2FB%2FeP45sqcYHAxiAmv5PXl8G%2FcR5mmb5ih0F0gCjfLizoPwZG5jxht40vybl9Yiau3NuuMqROlgD%2BhOBAXMSVATLViHLfHlk89hiLCb8lbMXBYzN4VjI%2Bk%2FfpycRhRXqzZkvQT%2FBCCOw7GyEvvqTRZmOv8G6NaZC1MSvYvmarbqqhW84qk3dIpC0uE%2FOG7W9zMKTU%2F8gGOqUBQ25nirfj2L%2F6R7WcYaEzKsyFYK67193P%2F%2FWCax2vAPMF1Xq%2B624vZWZDb5V3Sq0ZRHeKYbiqMQp0k1gDS6W4W55w9T2UQ9rzg6kMHxyLO2d9jAVXWfWZ6Xx6nyUKDltVPlbIienzK9o44EwU%2BYfF7mXwUZ%2Fb5ztfsfrtN1vlgSqesHhqqjA7VQQPKaPz9M%2F3gIpAdbrGds0QlNuJTRfSsP0SPigm&X-Amz-Signature=7547b5f19ff28fe5aa68e85881f148aba174fb20cfea1a5de3e4d8d4b04778d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

