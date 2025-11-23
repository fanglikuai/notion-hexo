---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFLCBMRT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCGsUeCXl4%2FAm3uEZef0xFWzp9Z4Z2jTqOMAjrViVRlZgIgQ%2Bu0LNhVYKSnxiDcS1IQR0v%2BZX7Sb7uOZ%2F79CCFVF5Yq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDMhUtp3iCNtsxdUJ%2BircAydM4ozFaywhs%2FkObDjm5T3%2FK7oZM%2FibX6r94arFs6pNgOI40zu5%2F2%2F1aK4geE6rrBE0o3GMZRJFgz9SN5Xjck44C%2Boym4EfRbIzxoW5TPBtlvHtfJcqDW0mjIdmBookr1Xli72n%2B5wVgMRJtNG%2Ba1qRS5VjCF7DCk0MH206Bo%2BfunAS1GwasSMLnjxNmfBwC1z2ERWKbjyStv0%2BHLHrdjFuXFVR83Mh8eFo1u3hK8HjuCjU%2FxE8ErhUvk49VCAuyHTC9hhUciuMTWdfQpguRaEF1GT4%2F%2BTK5beiLJ4sRkZVCOuKb6gKtgOYcV49ocB7tdG%2BHk8NZ5le7kpEp3pqFXP1AcUh5%2Fx0HWemIXl5c7N3RXCVvZeoxIRX0vtq2%2BUyoxdKvI6SYbx86lE2dnLAcQBHRnlLB0v95qUGuDrVEaqEH%2BLxYeZn2X%2BbwI6%2BWTAOEoGt71kGzjmh1u%2Fq2MwHX%2FluColN2SuYfnb5nKTXmohQdMAEUvf8TPcRMihZP0AS5yZgZ7wXR3XIc6X9fP%2FkLLE2TPiD6qMWLEOIAMoqhSQ%2B%2BpcC%2Bi%2FOdMdKCAROXoSMMgOf9lDy4IKd7E%2F4GtQVU9R4VPd5w4xZAlSBNWjJBF8rCnXDOte97Zn5KtxIMNaXi8kGOqUBQgeIdgbEgm02TY7FmJh0cGbyPvdvtBS%2Fj1J59mfIWBK5Hmu4N3UiXhevw%2F27iKVblisDtR6zGLc0AXZ0nt7ERfOLPzZQOi1J%2B7T%2FyZdN%2BX0Pj4V5KKlZLRsvhJOHQGPOdiRBcUMBcL2UmVwcYRh%2B8lWo6zE4SRDCZpysuRZHMnYnQqy4XA52%2BrdfaQqrJ%2FaJr2yuhltU9bxpqLmjbfpSoF6kBoH0&X-Amz-Signature=efba4b48d7c6d0939117e8d278e665b0e23c32577ad61b9c7d2fc170e2349a2c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

