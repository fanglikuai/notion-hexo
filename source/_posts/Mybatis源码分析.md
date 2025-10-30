---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS3LAL74%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAW6AkXeOtSQvvWfBeE189pJzfx%2FIa%2Bmy5oHmtztU1C2AiEA6OvOi1kLRFdT6BP3QmS%2BT4ih4CH2lka%2F4JhC8qNBtQoqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHesuP9ZdIz43nhpnyrcAyYVLMTtjzzx1HIKn%2BgmRl%2F8dInxpwgGllrKh%2FZIjuOZqr%2BHgOLFB4HayIQbuVs8AG8Ad3LLOo7dJ4Ip0GJNTz99HGH%2BGMnUvI7bvB8R7Lj0fr1xuijbU7p0iL8s8k6CNckgvpbfxIBhbUMV%2BMpRgRSlyxMDGRfChpH2j6Znl0YjTeTCAi%2FEh4264nJN6Iw9hAFmUBNRcCrKyrTK1m8PpGP7coMpCamD8clvqOT2ruwZ2V6mNjY5cVdngc%2Bpmta%2BPUKh3%2BZ%2FJf2x1Bb3qhN2N%2FQr%2BhfYnk9HZ1T0ex0WaHAs5Ofnx%2F2ri6unTiapDj8vQyPVQ0b5d3lPsJkXb043RawnX%2BhhCuclKDS8%2Bxgbj9Skh6C2POP57iTSjx6mSE59W248evSYID%2FrRZPsoIkLhtv7P28HrBd%2F9%2F3RkcPLafVt7wQ2ErOSzaTE0JCEuvIUl7B5viRcwUYQ4n31goIWx%2BR4mZeiwuVEiI98ceEC8eRU9AQIO8yEhhStGud%2FMt9WohYdFr%2FRByQjlysZlnY5xBCdyl511SyYVV75oDNTJpDS8pnvy9jJnLxjXtuC8VNEhB60UVV0UBa%2B31K2GmQ0cn0t9yIg5dYCn6R40FqlYtS4zf1A886OiGGmAid7MNPPj8gGOqUBqgAs36rkZfsss%2BUk7QoEmURCfipYsIS9sLHNO0p2IQg5hpFwz3raJyClfjhkHzOy1wYKms8ep6zz%2Bt1bepA9468ERtP554%2Bww5svI8gE6NBRgUso%2BLLEZnpPekCzdkSrxoj9CqWuHtGlZU3r7YUUaJdOeBGxzfYFVGmrxclOmHZf3D3FT64vdYTsccWRiezSk4D%2FJ8apScudRjywY7k%2FvSVlzRYG&X-Amz-Signature=29f2f6eabe586b3b4cce492bc2e1671c688ced32164dcfae950ca68e214cbc5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

