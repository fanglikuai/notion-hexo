---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CUSITYP%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHPljDJn6I65VXnuYnVLlj9hVCpeJ%2Fn9OLYNHc3%2B5W7QAiEA%2Bs%2FmA5u4cr6VVcHq1eFTohsy0vN7DCfEXNIcsYhQIL4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDKa2T1LuHXlMsluNACrcA3lUkstnzE%2Bk62%2FJ0GTAEjhoeH2mHHjoGrHse8xFK%2FOyXKtceG%2FB5yZuZ4eRTa4xoY50E7aDcVmhUsCDivVeDG9EAR6zpgzlXL4aTPgDzNadbWrXiwZeYl%2FA94POvZxUIdQCzX3TX42UD0lh2pmpbFacmo%2FL1TqrRqqsAPvWagmJSltymVAOeaCuz85%2FcDpZ%2By%2BT3YDfpwyYLYkosuRInvr16EjA35T83MKO2VncCn0AqaQsgtL%2FfN0MRiWSx%2F1vTnjJRoYYXArVc0G%2BxoFnGkh1UhzPu%2FUPLgN82WTFQ7tZ0pxXIRecsyBgR9KtklncapZQi1prwPNiRQdiqYXaIi%2Faa0P7jHdQgmjoyxXvQoLQ5lHFX%2BJEftTJCdRX7pfTD4Cze%2F2pBtv033sOCmup3w5nx5rHgkQM2J7wK0dPW%2BL9uY4khZD8u%2FIAqczokkwY%2Fa5gWwIX5R%2BwXSDo3y6ndNPU3UHLG6p0%2BZSznBmlCnqPmF%2BcTA66VNwicsaihmz86LVXf%2BQJdtzD%2Fb24aDM20aCZvPwaHNfpJJZsFK%2Fw%2B%2FDVSbem5AmsnNO%2FRhHy48fUf984Q9Ww13r4AiMzcww2Hda3vvAyJjEUVztMmUs5Odp5nAoG7yY%2B76kVRSorMKL%2BxsYGOqUBnzbBOZeZZeDRBrHKq8U35nD0Nkg4T4ZW5jn6Gvg827zFVLHHgeWXUfVfSxhrUG8hl274KNOvBrJQc%2FMQNweG009E2k5wi7A1dCW5kttc0NOGLjWGxzSPGSQCv33dqeyqOQpJ28oVr8yUpr%2BddVrAcIJ5tr0%2BCekEkztmjXoZe1zh5AQL%2BNwIJxwmQ73GT9vzWOJmAALclBhA3nnbTFbVBBpPQfrS&X-Amz-Signature=a4b5dcf9bbbb0821689b9dd16e9d961c77b8d34fbae4e0861aaf677bf9cc02d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

