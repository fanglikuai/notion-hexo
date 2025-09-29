---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656PNBO32%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJGMEQCIBK%2BL6MMsFrWDsvCO1JDB%2FLPh%2FURdebWCmP5e4sT1L7EAiB3kV9%2BAYLnVM19h07waZ0UUUQMtPjhKQWPWho8GGpmhyqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyqC4rLvERAL4KuziKtwDoHWqPdvrEJNBnNBmJ0GPDLz0nafVJCqmCYae5nR%2FTcQVb%2F0YyL9570VgVGGIZX1V1KG3nZnu3i1pRKvFkmuuOkuaPEvEzitUNp5kiapSlaseRCHwBSy6yDCXpW9daea5%2F5n%2Bf4l0xypgp7a6NG7cag32Olb%2FQ9AdpC5PtgAoQtCTfKgce6HCdRKPnyvHjUTbytHBr4zAU8clrXIUzw7RwEs3z12gumBXLMZlPVQkPOx7OEujBRYiwlQA6NrL6xHLmcZg5bufcCWxa%2FU3L73Vmvy6kTuChBsOt2PsAqybqbKqN43zjn2YX6Mlz0gKyKyMYzyYWLr72h20aVmFztXFUM6KWh1vfhtlXgsM2baVKGJBgOJ32vcq73O6SdUSKgeyUkRQLyahS%2FcqbmnUgSWU0MKM1JoEqdbFZiLmp2Vc%2Fvg0xfQj%2BxduqM3O8Vdk1ky36RLMa5IP1IEbaOeBf888FvU1KKyCAz1%2B8YrX3vwzSzL0xCclGj6jGhGCSrCvOb2i1M61NyUoyOSnWdcB%2FR%2Be8dyKI3Gins%2BqCoSBy2PsOP%2B37z%2BK2mssXVwVkINKnTfLd7p%2BBnH%2F%2FRkg1al8ij79N5f8%2BKAZvEfEiX3vSdtXSRJy5m8c%2B8odVXun7Ncwu%2BLoxgY6pgH%2FtY%2B6wxthb0h94QjGZ8Q9in8tQIslm%2Fv51HZZEs%2BXpqCu5iDkRRa8hPxHF4%2FqVdTWaaQePxYc0WqHmztMfD3BzqHkS8NKMXE8eX%2BKhMKUnuVntb%2B%2BVUwK8u7AOlFIHh5PKf93ZD7e87tou1Av6HSxBBZLkZmPCry%2Fhpcc24LCy9pKn9TKROc6J%2BNj4MUHBC6G29lrvAkCEGFx9TBBtnTaNhJ3gjM8&X-Amz-Signature=020fa57f9484a2199b8fa4487de0f8620445fd27a7b9de55eb1c852eb5266851&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

