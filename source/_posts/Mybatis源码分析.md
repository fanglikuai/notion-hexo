---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDGBXGG3%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIAdd94TbXSBiglM6d7C%2FmY9vs6RJTvouwIxhtpHQHL%2BjAiBaSMs8X7S2NHvAo5o%2FQr7fAlmuBsau1xT4HQomIEIIqSqIBAiy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRksZ%2BWvKy5KZoh3KtwDG1xXxxc61GY4uIvsRvcty5mYNy537IaCmmEqZVjJbL4y3ZPuri3DS6w11MTWyp6FTvGLYaEEq1vXaDLU1IlraeypkooccdR1FkE5LYl6gpyU%2BacOwnU%2FuVkB17beQ9IJ0W5WzK0Mcrp7aM%2FIvkwbQ7o8y3TMesqhljrq2OqhCtVTh%2BmK6inXioCGyGAW6BkSzakIXDk7Y%2FOJwaek7HjUzque8l%2FaFUBfYBUGZmZElZ3NsrDIyiGvgk7Fcoc2z%2BfPtHh39pdjYFZ2%2FEw27Sg07iMz3gOy%2FJgH22PvPmW6Vd5rc14mwS3sKNp0JIpTdzNzDMFQvDKiez0NVH3EqRDDgHK4mAYpzYArLEHHEVvvTBNbNnN01DHtqJ3XC2ldO%2Ba9Sd9yF57jRS%2BMOiq2ZzFCBJoiOcC1Vpbbvy6rcxD4J5P3NYlFr7ScWlhMwUtQXv9oNP7SFKdSzx8IPlKNm%2FJycMNjtEquEl4kZgKcWXeHQMO0Grp6zA2hy%2Brfqoyuk9%2FLZh6ntByEiwZvaA2byMVklWs8blnl%2BQkaJHdr%2FdhJVOEcPIdndivk1SytBjKZIHnSzbxyZ8xvITBhwrHP6ie2W2exZ9iEh2yclm3bDD0bBm9FHm6brf5uYnUyIOcwq7%2FLxwY6pgEorSd9UOELMDqm%2FIStUpYOypnsBewHvWiIHyOHirxrUq0vpyL9Nzn9fWXVm4ahTBiu2n7r%2FEuDLKxGw3h0JBYDYKPur9Rx7tE4heLMLYtuT4mj1qrf1gW%2FbeK4lWir1qOmuLM5FK%2Flz%2FivfpTXCSVhNnbPWZpCf92hv%2BHMpvkaXPQZGSz%2Fd%2BKHilUiau%2B%2FZBSgup9tHdwk74%2FnWbYJNez%2FELLwXgA3&X-Amz-Signature=44dad2afaedc94dafb7d25ebf51fe4de2721c780d3f46cde3d86bed9ef12a0d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

