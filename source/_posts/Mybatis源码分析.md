---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDGUC7G3%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFwnx54920F2HTKqBZrXx39WhaJ3fAPMzicevrW9x%2BpsAiEAp3drz0GUHXk18G5ExxC0f8Dq0XPeCtHAt%2BPiwttw8kAq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDKY14K6lJgIhUsoaByrcA9Sy6HspznmLtsTmq29wKkKnVS5mJcuZFd21SnTCEjtVKZN1xF6Z90aWPX0dmlT80UNjYGWsuDf4RoWyd7wfrsDc0giCNxS0FwOw3lRfYMG2MW5RWKLT0FSC2suM%2B3gebZzdkizq%2FNq%2FTDTo%2FuIzqpXwcMEt0gvUEhbzc3Vb6QoNG0MXQ%2FHYzFN9ZJjua2FTRkpWPCiGtao9UciD5VUv1oAdWzIo43OgC%2F2g4GOrB3fqSXYIA6QYpfMIN%2Fjgx0X5FBqueK2UnAJiHqpeeWKjt0FkWV6J7%2BzG0tdJ%2F3p%2BTKofuOv%2BSVPqBPTMAR1uHncuT6eAV%2B80ZaWoVhsF74FTEH%2FRXOjgEDmjO5dDExSHKU4RbjfhrR2D11nlzDDvC443UCmYEf0W%2F%2FP%2F3tYm%2B2DKPFxaCYNRnoF7iA4eInZE7AxNowmqPmqWamQgdKKkvIeEjLUrh3H6qTnrmra%2FFyCJDeRUbNj5za5uDbVnjmUu%2FvZjzkFIOuQ7nJgdADxWcSMk3ZpTNAz8F0KawlSAArK9fb8J05orFWwqivhev3xdHhXF0fLIazYeV2XTb4nzJE188heUBLeCxnMQVbgbVKpSoYQ03OyhCxAeY0%2FTqBbWU6FfIG%2FiUI4ZelYdcO2lMJOQ%2BcYGOqUBp9s2EY7kHZQfl%2FVNxVE3LRu8PpBhmzBujVzNTvKeMtrhOshd0Aa7qKPuARHsmDjbvZiBbpJB4yP5582HN3sFbNL8ZBO6aVBuMaxLVMzaz8Jo3MZb%2FEV2lYsz4Tpdk%2FMLoMOV91jmWI5j9pg6OdQbMSlNto77ZJByJY%2BGAn0ubmWmPBnlKUOAGSYN5IRvB8Y7eYvAyvqwxLCc48v94255JCoPT5Mb&X-Amz-Signature=958175e4ef5bfdce95129f948da5bddeb1dd09dcb21d95d64b0b141c1e5c71c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

