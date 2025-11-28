---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662NBAIAH3%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICM6DwVY7S2ct6FeknhdnA%2BcdLJgIeQtei2TFdnf7ilsAiABi5FFiqBg4BZGG4aW36pUPVHpmghhMr7RhgdWn4gIXyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5WCj46lnq8eMwhz8KtwDEz8jQ%2BpWiNw2ccxS1LWsUtiGUgUeBwG7C4V21Nf4ySoBF3s%2BkI0D7MVx3F%2BApLxHuB68ZPWdrBYK23vir%2B0BMzIhIRGacFHL8t1TNKUnOlnNFJcQLKkAsIXWY1iRpixM6PIHfKTbboaAgyDnivtaeB8oSsa%2Bp14qYj7cVnyPr8Nowc1gOtrKNDn4r%2BohRhAryX5Hgx7UoFWtbI5dVMgnVyzDWFInWc5plfHcaQ7dHVlwzhjcEXZa0zMaOuyWD4BpLzjQPpn309quAyR42BPZugxVcyea%2BRvXuMZrDjtIKrIaEN4YoKOIa%2FGwlQowpMUrpDUjJlceTaT8uWff7OKbUSpamgm3fuLVKN8Uq0kDL4L52iN2h4FgWKr%2BVSGBiYu9f0H7hGBsesbEz%2Fdm6x%2F1GLczFgdpxnYVRxePnWQoYQ28pcQZgVXej0rx8Cm8gf8kA4J8AUm7cs%2BAi2%2B19BUc36OsSQEOqEF5sw6FF753HSaulHRULzjeK8W3nC3cha7YxJBIzNQv4JcPwTZlIxBizgfLLLTWMiDW%2BtkbAE0A8Rn7a3wbz3MMTOWPbSxCsNH%2FTQ%2Fqt9%2FrWu4k%2FHJ78IqFz8jSBBdznCOl3knQNgrYnZhurGd2jj9C6Kp4EHAwnrukyQY6pgFARS6jMyGd%2BRK%2BycrYtO6fQYuQjbI1vEoDQT%2BAUtg74W0hO8RFwrbo%2FnTiAAP0VX3nM28fEcYwxGBw51SDiIf02OTKM10yX75EJgeprNpI2NtOAP3M2hb0iGuEg1rb%2FlNo%2BP77mjYZ7lEhh5x0sEhSvZMxZg5oTUq60bLqZhXkSNvtQtUYI1m0B61XW2OeKz%2FMvkdQ6da7wyZRkSC1%2FycVS9o2vrgg&X-Amz-Signature=02f616b4b168d6b26674fe4fb8ed9b9c105d7f2f339d80d0ecbbefd7ff822ba2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

