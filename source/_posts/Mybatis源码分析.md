---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG7ZE6LT%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDMvqZeBpApyOUhgRIwAvQzlB93TyvbnfXSDzzcwImiNgIgVbLEvyAD1Liydz3zewk1qAPDDJowHNWli9I89zx6X6kqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHcJvSNyDObKklb8bCrcA07T7HBVdsmYFe1Rqo%2Fsk%2BFNGGHRuHnNfPFtMaMVcqYXW9%2BTMKFAcJyxVOcoHSchUN7UAlKvibCbu87sINWqGleIuzKc8RJLS%2B7Yadprk9ewkhRR%2FzFoZkOqqhqcd7iRZXwRRzoyH5jfhQurclYt6c2Tz0H1TkpnblxArG454RT%2BlvyMCkJ%2FVdrvwzEcBWvUDG78HT9hDBGjoASv3j1pyhbotADr8vCWtsyrhykEQOCuaV37NJz3F1Ews8bqrwEN7g9PhVIzaD80yRoJZlv2N9emPWGwMGP%2Bjf6HPunrHFBUykzMjJVoO5UEscouU8lkkbWBRtZxUEMLVnG1Ug8PXTZ%2F81wEwcZkLYaceBCB%2FSuAzMYx%2Fm0o2F4K%2BVPkvfCIDJC%2FkcyM9pHjpZs2GXQn3HUEwCXUPX9lwJLjnacz1admPzz3VwFkbhCZKVCCTCkAFq7oFfkRJsoPk4fwJ24iHDZD7njrzWqfLm15RSigRhZZ0V1eS9dEWGKs9H3X3wwigT5cqKUnj5%2FNTHKRuLMHbokIBu400kJ0AqOQsl1%2FkURhiM8CORNSG2qnmkcP9xRhUe5B9VxBywYZ8yOBMLv%2BR%2Bmnr5S2K3LeS5RtFT%2B1ZrkfXSHKBL8VSUlbg16WMIONgcgGOqUBA91r7ehgrWsIjUACo%2B5YWMTJIygV8XyUz0Y5RJAg%2Bgm4wOw6gCytvMtFXIDPotrS9jsqbHNL9O47lCHRA1Dl4421zrHxSDuiVTcgPhTmwMl98bmJkvu%2FOAtDYQ%2BjSxN4LtjuYhq3DBuHujvTorGoGhs0CDt7EK8IeSjMG6hGIDH8fEz%2Fe4UTEY67%2BxOBZtk2Piuczl6qZ1UQ6QTTHVr4wq8%2BREm0&X-Amz-Signature=808b27731a309ad63ef898b79959c45253822df3e8c025bc83c8fa946a07423f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

