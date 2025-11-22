---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XU7R5ZAU%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHJqH1Up1qYDgFjdxJ%2BU%2BLQNvbMTz3yRyTuSaoQHPYy7AiEAvlslvcuydaRhipj%2BPuKHSmpse4v0w3KC1%2F7QX2QLsMIq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDPH%2BaC1xdGFJvAnk7SrcA3uYcftsIW%2B%2BintdC9KvSQ1E%2Bj3hNBlOP%2FUHeIgQvTYmeKTMkjFJ5RwfAXGi79Ve87w4KVCzZBxyEjqETLlTQAur%2FJ%2BAid3NR%2Bp9V%2BAnHQnnUnN36Kt2mbPJzxpN1rpGTjibq%2B7JsZiPJyt4ErCm%2BbeXVl7Gvwqa7f1O%2BpLs6AkfVrNKeA9uuqTmRALCuycpB08uevQGUDE4y3hhNwCfj6uWxHckZvVzvS%2BDPBtVbG5gGl89%2FISdpBGuldOHnDfSG9EXg7FAPepaPb830I9f70ZIjif7G9kzsRSOACSVOsgPolGFNprLI4rcFWbtvz94QTBxyJpNHYoHHwn78PMrJj4tcJlCbhhXuhtwHR5d6WlYVaTcwPIAxKqiV1jsQilJhkVXwjf1kW05DzDAyDQ85t9D2ZBzGTBtyEVulz3lp%2FUx5sYIbV2jyN5LiIZv1Ke06aY2Wo%2F1YrcZBecIX9r4TcBXxDKVEKM8lvCVN7MILw53ND8oa1kRpufz81qeap2lNVeStvYEETJOpHDpp4JLC7brJflpfV%2BSzl5mxiJvdUImroxWo%2BKZNBNyt4iV7V7uYHf1MLA8gH22xTmNDUlsjTvtVGFuZczU8GzqwIfszQGL1h4uRKdgo2%2B9GzumMNPKhMkGOqUBsmw%2FtsXFhE198NZVbwvN7rrEMsDfwDBVvyfARUMKeVkJcpw9TKGJzCFxLFunkaaf7M9TqxLpzE9QiA%2BEBADY4SSn5lXx%2FMajmgmlP61cKHwGKYJYdfsk3T3YmA0XBmsgrCzVNwuI0vJHN8PC2bOp1JV3Nj%2BNoxhZxXcjFiNeiwNPYSFjgJUgsF47l%2BU5jhVH5zv8cd%2FvZESEu8EuD9vZGn%2BuI8xU&X-Amz-Signature=733112e683e5a732ef9696aa869efd9bfb052a427fcb0338f3b2f119c2496ccf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

