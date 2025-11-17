---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YCJCHDS4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDWkmXgyrBwtpOwsBbRUED86wpdKDiUbZcy5IUgujWoRAiEA%2FfCUnzXb43smiiBpuTsNu8fZm906xbPbQFQ7BLrEni4qiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIjVmYGG9oNhlzsmXSrcAwTPKsmPE6DWye0B9o9RqK3%2Bp89EFiZK5zogZwbWoQdnLbW3zttlkoLhQMbwR%2FZgIAtJLWrb9VH24cYKSxYlt5Hgo3wkukgAjf4HgEN2PwXRwHZ%2F4jYmC0N89K2yF3in1zG5K9RX1bow5cdNkTEqNlXn0TBzX1NlhrSyUYOfdoJvnpZ6yzqGQt6RGSY4HD6BkRsWhWAU%2FN09nKoay%2Bdc33ml09Hi7Rm8NS46opzxAznJcxIq%2BvFjpvk7mz1I7X1oyO70%2BHpOf1354vmrhcAcrxCrQl6Mv8lwQePUOvSe5d8%2B5QSu3GIXfR2CvzTHHP4eHVcG%2FP3zAThfURBqdjjVaDF%2FC0RIr3G22XJ1OcNYA49d8OcjeS9R7ER0jbgfyv9eUYVFaSHjdb%2FIx9Mv%2FzmGnk5%2FuoxMmXP7JKRfMKw5Z2DVZrYtWD48Uuic4iZk41x0Yjm4WT1Su8YZaDS6FpuVmmK7IJDt2i5FWlgWp7gsqWi%2BdhYQQAZ%2B6xs4PRV0Kpdb4oD4N7qdzXgR6rgDyQOOSbCCfrdO4AD%2FEhOSapG2GZLF7g13df%2BgaRNZm6lMRILeZfjdr1on%2BvvVkQX6AU2rahQmvHjJHqVnA86906PQenfr0gO0H7tUTpT3ThCGMIeJ7cgGOqUBW6Tq%2BL6JdGpq%2Bd0Htqqs0sRIeJfWUAYl8h1bQqI%2FK1lAGezDPUpNf785HsNP8i2n7WNaSgbcLObhqQchbJxf57BHu%2Fh4qd82%2B6BMWumaKB0OLOm0kT3VZTkrvxO%2BHbMoWjAf7Q8oLbjwkJUZs%2BhpXJLJj8v9C4VCv7osX14sBctlrYniB77%2BzfGLskdy%2FpML5SnPBYsYUZ21WEbzbj8qxFUfPTHw&X-Amz-Signature=4b3056cea5c28ac0518926acda924c522422ec1eb41493f471a751f4c910e4fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

