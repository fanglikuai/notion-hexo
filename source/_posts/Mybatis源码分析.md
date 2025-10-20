---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q3QK5TKV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIFuqSsBDCBc9%2FhWpUsaAsTD7XOuhCeu4RfG8WPwPmzGwAiEAgMdgWuGiOJV%2FSaKFwc3MdSpAUO8%2Bw3a7hwZIPaYGpncqiAQI9f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDoYQMWGfWiA7IwWZyrcA0Ni00TGWRwdZwHk5T308SyIjGAbvmjBIYSK9MOrdZFTqnShhURdGpYFZ3HIsAe87U5kc%2BE9JHFF6Ld%2FPwSzToqDeM4h3bf7V26jLDpYpwgPaB2kiuX5%2Bhe2%2BMfpSoYZO0WjqjqQ5uozbmG5OLRn4s8zD%2FeUmxsPhd1mIbcxrJfsgi%2BicFJf6EqAS8YD1mJq28bG9wJkSj%2BdC4njPPwiK869UvqNrWy4l9PflZmMyCwQ%2BnXFZFXq5uOLdSgVvsG8qeZTpUIqQxwDZZtXnNJJDxpO%2BEL2poesaqUgoMRNDIhNptv8yh142vHwSyHQs98sc7XwzsN5P1DH4za1t03QvBSofeVC7AOcy7EzccUV0WVEBVjlIrD282yuV81n%2BAbgK6K2H96lLRtPjL%2ByPz%2BrkrkMkwAwKOuLf5ceGL2Pjynw14hPWZD4KqLUw%2BQINqD%2FScY3tkN28NQusditW6rp%2BMfQ2SB6Q8yefCJSH7GSuwSQiPSE%2BCHrIViHxjVmw9hngPZYrUhqttq4c%2FH0FATW%2F%2F6XLlwj2nf7LAXB5G5D79HcXP9nZsGG3ZRzTOclEZxgfx2YvGuukmaJPq8YunzDoJxGqqb3Yi%2BS9MA0r1NMW6oXMH2aXsf6XF1DtkTaMLWc2scGOqUB4585T4lvUwqqqHThQ6S1MxyWyagbRZ940jU9q6TWePswPZURJD6IgL5nDEmkBgcKmgTwcw2KA5PLbTfPtLPUyA3Q5TWHJMlXoJx%2F%2BSulPXirEsBodK%2BMzf1%2Bl%2Bg9I1eoN0SE48UVez%2FQgb%2ByDYYKw2Jlks103QfxVY0jU8fjpHD10ilUedW2M7VBfF4S3EFqz8KrVhfUJGYw9xOHV4kF9Adz7muG&X-Amz-Signature=4625b90031375f0affc53c81c64921558d09f820d91839749a10201b53e33948&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

