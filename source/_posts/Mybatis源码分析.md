---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OR3FOPW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJHMEUCIQDqZ4DFg49ql32yBXfMZBLb2ERkQdppgaTfQGVhSj%2FuwAIgNjeJWmrPAFJrOEIJ%2F675wXwO%2BBtE7rggaaHoqH1ajogq%2FwMIJxAAGgw2Mzc0MjMxODM4MDUiDJ7E%2FaBC4h%2FB%2BRXIOyrcA378avRf5akymzWryEk31vj3kvbc8dCc%2BjS51qRN2TCvUDICaGFoc94Da9zFA%2BKe0jafvI%2FLsUZmGfC9tUGOHovxScKzejIXR9%2BLPQhRJ%2B1CHaZlXzF5HwV5EzZ1G01p%2FbSo4ezh7bOfNkoZES3uKZZ594T3wK%2Bi%2F36IMFsvHKZWWucogH0mJLUQkwhiGPjj%2BIdA4WrerfasxGMyYRHcbuDStITkfD8MmD19tlMO8IwF1SgsLQ%2F6If6gwnwquBFXXFkPmx1flOH62Bc85YczmtN2vXPZRXzv%2BISao3RH%2BoIC9vE9MtffGxmKp%2BsSd%2FG64Qb3bCES31ZrDPyKb4pfr6D59Jyw%2FYlcr5%2BU4m5xWljNrfOEDwgm1k%2BKUP9MHNJeC%2FhNoLEF68hEVBoptHg0rCI3ym%2FWZcw%2FUAiMgOm9N1lWidkY9M2VWl9au%2FpI0q0O1jdMV9GPktSRMFhmecqO%2BLcUg9I8DtR8oMsulfYfpydnozYu26YI2BvbH7bds6COO0r10hYh%2F5aAAxmCvvX%2FTmKZS94ZhflwmnbwzlDSgrwhvp7SqogsD2nPXiJUoN7NUozmfvVt45IeXzbshJ7OWB0ljrNc04AFrcxMcpe8VVFMCfA%2FnBvBYBJcBH9iMITpzsgGOqUBx4%2BFGIk4%2FtKj7RVEMHHjJtNGDokjIVFmKM2M%2FFe38Df80XyVlasP8v7f1P%2BMEOAEzZLY47PTh7CqD1kxcCVCfCEWFf%2Br%2B7oh3%2BAlQ6Y6wRATZSOiR%2FarIkuZ%2BrwERwKfZiKp7%2Fq9VvzVt%2BVnE97fHyPA7XbcKu4doVWtBCJLH3Jc1hYC6t%2Br9Joc3%2FsZCX1BTIp%2B0m1EJ9x5g%2FheKG0YLOZRvAJ0&X-Amz-Signature=c56efec3b841cf610040adbc89c7e6df8bea796c63ba4ac7248697021d46427c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

