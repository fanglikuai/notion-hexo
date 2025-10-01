---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBH7EYYK%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIAdBhKHJoOMsZVRiiV8hg%2Fq12ML9RkgYUOsJNtamnYw4AiEAj2rrkZieE3Wm7Hjl3GQqT6AJfhp5Klk4lQUclIW2F60q%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDDXK0iotR8ZxDjzzuSrcA%2Fd4ri%2BXBikK%2FkF22nHUdkTa77a6d2mhMeuwVNGWxB7bnktm66JGMGJ0lFVW%2F7yku55ct5LIZuYJWQSWt6ysgXQIJ3kmFzu1VSIfAEh8oPvtmOWZP0e5AcloRM7efUWd9JAv4OokLl5be%2FnreGXs6fnjvguxS7beipjkv4snEN2V27f0QHGTv%2FF%2B0mqKa16K79BenboQmqcX%2F56AU3VwbDtdlmCjjXWn2eyr8iZAkhWMIe5iJeGaJ%2F9AL5KOv8o8RY%2FKkwJQs0dAtiYAN2d0PZ%2FTCHXNlmlnSF6SCRrvWOMRAc61xkSS%2BfG3ozN%2BGbWgEWAvfXR56aMuipdr%2BjXqapGGWGu9o0r5SEvQw6UblzYBh%2Fd%2FV30OJhhBLLs2Z1pdl3AIifZI92hQh0zOK90qWKEXAYTjR%2BCTsexaf4M5NIn%2B%2FPabcajpsDkZWzPhJEQngZPqCCYSQdYwmvRKthSRIvMkbHUVAcj3fiVpAT0Q3u6IkUdzcsvtI8iv%2FjSXOspXj64czl12LX1CHaxWiFVOb0L5wu6640i3kD%2FfTB8sy58uhLQkLpmzJDom253svOKA9NCfnp9GvM4gHsOrOUxherX8UqF0EmFBJYcjs3WbffKEtKVuluQ4bpzjTB2XMPj%2F9MYGOqUBdfe58p70IqMMbyMgtTFtBpjYyP3p4r43Lu4aMQKzV67tBTy2m%2BkQQhy1QbJkFYlQAYpYiL19EkY%2FweD%2BByuq6kiwK9lcDgliHHUVlMF%2BNWGWxIYciLyC0jmy0UUHO4wpT%2BmxTklVrYvc1%2FgXWgTwJeZVVm%2BfAxw2ojsghk1S69BVMyXw7fEHYTOpuT8GtuY8MtN9y%2FdAHuOe9Jbvx9VOWS%2BY3MFf&X-Amz-Signature=2e303c72d704874f1a65584044f711c5d33dad5d12df97085b84557df8d6f1b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

