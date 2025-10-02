---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QL34LJNO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDBr0mDCdxOt8yg8SWF0alHKcbngywBapK2lBtcNAowUgIgIMoNIkgmJ043%2F6LxxdAB90bMBEMAw4CACGqMfq7fiNgq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDECLajrhsLhsv5WAHircA6KAuGvVRYNPfcUgnQvL%2BHsSZLHNnk%2Brkm5tERWpa4slXGNnlJdZ2%2FKjMMD9bGVoxhPD50Utt60GLDwnt0WaxRVPZrDBlnRZbKGSF6FWWRaftZpYcpvk%2Fp320%2BN3eqmzvlPYQ7MU7yvPLP9Heo%2FmMD%2FJkLxxjP314QtAAL6fQD5jlnwfl9DWO1PUfDMe6GKDQh0KhqbgqyIZa8UXKFCO1EAyJG1mhKhhjAvbddLTiXKOYmaEqsAuU88dWKwq4C2AzBKIZpYbhRk%2BzU4734KDcjHpHM3peAZyWZuGw0dpQCofW25HF7q2oXQJZdS4Xnyt5LKE3ecjG8DARwE9MXFx4rZS3rtjX8A1kR5L2Izam7K2AT7hdWEEo%2FrXafl397JG3eG3YJCubLEYhB4c0NkFyj%2BVbztOch7GsPGpCOIapbKp5CYLpdNRkpORP5s4vTABD6uCg794MQUMOwDwcJw9rBD8KRpz6MEHMHCK8Q65DdJneoXlHWiTf5ufkhvIXgX6u6wiBoPouYQhNSv77%2FIG7H9X053T%2FntSnNhJ6QUx%2FNKWWOlDCtzdT7jIj9NaQKHwSrz0sLVCKrlaPLz3BngYG2ZM%2FAzTs2VG3qJzYWqUR93hCux1pwOW1BtXhBbhMN%2Bi%2BMYGOqUBSG0cZ4%2BE3%2BuPHVHpRg0iq1M%2B99IkHQ9hbt2bg%2BBWAkdT0Qa37VEkQ6RGejd3D3pn22vIjvm%2F64yZkNrfNAv9Z%2BVRfDEBFjhQxomhDmK76SuiCjG5W%2BdNSak5OQ5J6e9IeElZVLXeNYFT8zAOdvJzOD5lE%2BJDXy3kPuPKbvf%2BJ8q1p7TKKGgZ9j9GOyJNcmL7GpiILUF%2Bj12d%2Fld6h%2FBzmiSV0GSv&X-Amz-Signature=174eea0b7e885bd720fcebd600252281f853de658917547006c7d7df052566f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

