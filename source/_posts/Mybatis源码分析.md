---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF32OY3B%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJIMEYCIQD66ChR81H1HCpFATNveJQUXIwuvOM0Q0imYfgCmAMk9wIhANXbFKM8tHTA4wTTjWWM%2BNGmh50zAfvEMV64nWgrMxMfKv8DCCUQABoMNjM3NDIzMTgzODA1IgwNe32jIFBL0wLmZVkq3AO639MWJfPfVbqw7WBB%2BxRozJuXcq6h%2F0wynlITeU78HhAUG4H02haTCPRNrNWU7Rl%2FksMsIKNjjAuD2bsVW5coy6zhW38ID7ljhRGLkQl%2FzJQy4sRInU0Yz7gvMDHRD09U94MHtBIfwcFexwzeo59bgElnK%2B6G2wOzDP5%2BRqQbqJWveULKuk51ptHOjoFHv%2FR5LLkhXoUcKgE%2BDNQjJYbSc44ikXdeH0z%2BSkBccOokffSxvMzXnhugU8YvpVDWFEksG4TJxCp3khdMjxNSmUQZi0yLk1Fg4V7meANfVIIo%2BbLOqUMzp9Y20dq3ETgsZDXnjvdWeQZTS4a7fDI0lvrB1%2BuHmFSbhaKUr65%2BwFIbzxpJV%2FESKCUMh7CDKzI65Fv%2FVjzyX14MHYtSS2zKnsx5pRX5kBJHBZcYueRM6ecSEwlOAbbMD%2B5i5L0DunKzLV9o0It2avBNPPs3qxfi%2BH7gxFKkzXsGOZ6VRRzQ7CsNPFdajDgLhuApCsa1tHPqEQKVgCy3eECxLWPU3bDgSiAmVareaMutXyXS2yPIJLk7R31olahkdPFS0Fk0JBbNjkG%2FVcVcyL5eCi2vzrQBUlt79q%2Fvw67g4B93DUJmQK6gLtTpEZAlPsllQ21JZTDOy6zHBjqkAcgJH2AP4ZGx2l5X2OQZBsw2vy46x3DA6b5fsWP1%2FnvAx4Od75b2NK2jdxszK7HRY65vQsJg5%2ByBneJv3EwDCJ2yi4HRIc9erduQuXSwUguFY8MIDcUTMEpf1yhSt6rokIWeAIMJeXr5ax2XYwyXO70O99oR44ylZP5QgQx8HlG90NqeMPao4I%2Bo7kgg3LkK1vLlFDJQbK9KqGkQHOjIMlIoxlYF&X-Amz-Signature=397583cda356606a92bc59b30e659d7dd2557b8217325de72af323c6cf8562c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

