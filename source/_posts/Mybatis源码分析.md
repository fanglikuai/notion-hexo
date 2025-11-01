---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVUGGHHR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDVFfJxfuU5UpD8OuGoDlT7rhlgXRpdY0j%2FAaxkFRq%2B%2FAIhAJtAj3p87NTKXbjA4HV3CAH1g72WVICq00h4TY%2F1AlytKv8DCDUQABoMNjM3NDIzMTgzODA1IgyNWCP4oh%2BrNgozHhcq3APt0I0NePVaJn1Nnij2X%2FttqbhrmQK9ltepd0kPc8u5Gh8LgwAKCPoq31%2BqjVGLgiOasIvCUCeEMcmdQ077P453D2DtczR9S6KjhpnBO0dbJKxKi9XGvyBbIPgGXkmQlLirhvW8di6lCZTDo4QGGrVOxZOAu5YQ0gVRrGG3IrlEEhYvOUY5TZSKbFEhoXGjYHuY7kTM3r%2FquGc0Wh7%2BxPUC3fytCcFjV1%2F0%2BosLBADaTCoiqyXiLjkPin75qGu9HNmlS8TKZvi0dMIP3AGSZBjBWgf93cxyC25lgyaOFWfbupXBDoPW0NF18iP2h8%2FvAUmxLA35%2Bb0LP4r2nq%2F%2BNEZofkSvsvVRd0UY%2FjmoDwCAP9zXu5r8GJIplZ%2F6%2B2mZOYOWyRj5KMjUeeRbZ1CF8pKkGLMy3j6bgrkm5HJVnaRTCvIfT1Bs%2BiMz%2F%2BL6JBhORnyCTTickAifNXSLLrviAfQ4eUMZbh6ScPgjYhMp3t3aJJ871G5Ajae%2FPJnhOSDeDfdmROyaYznYFN1KY5Dbx9NZ0bnHHlQ12oOr9jS5ngjFkbeCBUDBhHiM3Uku%2BVAPH3KHrzWo6dc9NsekLrSpjYUktTpx9W8fE9BVymLXrWbwMdSCACECQ6WiBfFE7jCYw5nIBjqkAQYHweVxycOJ1%2B%2BLLPAkB3AeMMR%2B6jOigy%2BkV9R1Nzu8SpcIWY9r3Xx2CLLrwxqd8PnPI7G0ze%2BVd8rX1cL3s5UZJgcHutzF4YvFBxUIzkeAg%2BxqH8mt0RoWQiYjrOqTCK3Pz94n4HFLig55TwuIGFfOAL2S68%2BGLYklBgtqPDBXbCQGvqj2VjO%2Fr8RukFKteb08yMNOnNcN7e%2BWmKJLlEuips9B&X-Amz-Signature=77c49883552f9a0fbda48caceea6e492c86b9df5a9fa0ce0176e55c42ab31967&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

