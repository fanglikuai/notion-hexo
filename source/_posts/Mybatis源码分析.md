---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZJFYMQF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdKFs%2F2bqvGQl9j%2BBDD8q3wtsEXeb2SPMBR45xvkLgeQIhAIdhV95jnUIO73zZ1urCOn4ICpwWCEkb8f1NoUoz3%2FcCKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwK8LdUBQQi1ncYUVkq3ANWcTUe5sUIwk322n9oM1HGzaL%2FuXl1O0UJsFMM%2Fs10pWGg9D50%2F1OrY6EiFugKVVACa5pxZxYnLoHq0ye5VIwcfDkq%2FY1J9H67Ptw5N%2BNWVLdEnq4ZnO7o0I9YuqURPKNYIrNQ5k1VmW0Ju9Mw4nO7oMZ7%2FZzCnxu4MetmudOkPmyOZnWujINOj6jzkl5XpsDrc2KtYoyq%2BGKc%2B8hLFABK1gZAVl%2Bi0PzOTZBAKatlopcS%2B72g%2BpMFQ6NuBktp3gcovXXz23KMijjfZj8hF%2Ffh7Z2FwwGC3F3OdMJghyicoS2DASKr7ozy%2BVCAIXd%2Fz3v3WtIpukw3U7hGMzT2%2FBH%2BKf54qNoejrMpxDB75Wyn0xJDD%2BSRK7isw62qt3lu0PBMJBYJlyRg%2B9wbIs6X%2BCFZtA%2FtyfTnHcoElebFedezTVeBjZu0HLIjbSkbViKoPqyyGi4kf%2F0VnJ8iBsMlNLs056DShGTZ876BFtMfwSsRpXc1TipGLSa9VXmPNnIwFCIgmtIaNCxpwc5h1P0qh1Lanjwd2n1ihuj8hZsOd9%2FSTDRyaz9inTgC3oeYIz4BsjjnKtL0LPnB0PwI2agtXl7Mg9qjZ22elg%2FmVuldG0GbvrQcOWi4mDHlap8KIzDWstzGBjqkAW6qKCw1d6dHm7udEYB4LoRuDKhX4rveauWNhGviqUF0hGS6wr%2FC6kS52LPSs2s9d7rdQ9Y0yApGEtmEDCZQgy9QQ1yAo%2BdfAuT4tkF9jIRQDEPqD6lhdUY1PbZdNqWC4Iped%2BEvuK4wvfDQw2mGBka64JqMORIizknmINwslnbw%2FGAq7F5HYfVxLAMNBVtjfdkD0%2FVsGObEdUbtqTKO73M8uz3c&X-Amz-Signature=eacde672ac0195be0c5e8226d042717cbd81571f29e10a137dc90bd1f3f0a19a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

