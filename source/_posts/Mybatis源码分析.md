---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQNUUFI7%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJIMEYCIQCj6r1FSU3zgamnby%2F2wfzt6poLRw9dpG%2FA%2BeLQEneB7wIhAIqOWbURd0FsL6cwV3HOh2OaQQNDUwdTFYbIDkv1marnKogECMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3biIusUuscq0yt74q3AMW%2F%2F25RuVZyKGaoilDtCmpoz5ODe%2FdhVprrhyid71h4PkOddDykHQaaPrEQCDxoS7Z9ib0En5g1DuBscFzr7%2BJu7pflDvHA5Z9yK5m6kg7t3%2FdAXNK12xEo0abZbCOs0eyJJJWZsCgEGdg2fP%2B%2B0ALqcbyqTvcLQePDH%2BWaBJrCXrwZXIPznYSmZvItI8Xde%2BYOA6rfxi9WZMcX0hRIyCErIdoqmdFol%2FBdJN91ZsaQrPxauhQlRi%2BfzQBsQa1OEPxZ%2BRJG5sgp%2BPpf%2FFloG%2BodqsJQ8pQ5lN8pKLiMD79T9oXLIfqRTmOBn4M1%2FN3EQLa1Xe%2FUAwkkfMOx4gftaQxBmIzKR2tjxAiU7M%2FwRgfrTA3QZ41BoIgutMcUyih662MKiprOGqd6rIY17BZEq1SIwfQ%2FYZj6eDPHhFeknCOUqlhl8vLdtbfJqEUcHn1zZU9JLnIKim1xykfCtVoJb6lkvB61kZKVHpdBLjY0%2BQjccxfVVIc%2Fbp4pLjEJrs2EDcQM9ySjpZa08Cq4dima%2FRYn0GqQogUE3IqRE9Y1zdbVFvmpB1cTRXzVgKbjc428dQzFO%2Fx1gHPf7TYI3VDZ4DbbwQXyGkYSCKA4sQIhQjepq%2Fn9CUS7xf9yp3bXTD%2F35rHBjqkASOUBLFw267j3Gpd8G7VGYxzxi0R3PuB%2BtNtsj9GVk2ag55AyMUKZmripQdcR3WRysZvNjPS58%2FAaG3B76x51pKEvkLTqCEdRnF4g1f1%2FCsTTgfBNRMfe%2FakmuyJr11iEQRpWhysyAK5avNx18BQMLDxSVyAvTSlmWOIB7lydAutHOM6HqDNikrEUZOvzMSxRqrF%2BO6DVr3W3un7avNj%2BmCpLvzL&X-Amz-Signature=728cfa05387df00bbe94dabb41af02c179e0bbd32d25849299c1479ddfdcee9e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

