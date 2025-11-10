---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRGLFFHX%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJHMEUCIEYYjy0cXdW%2BEMXpvA3sJSceknnvfnQOZMG0IMOLaGIiAiEA0UAo4nkWST4iOj1zTkzT1oH28s7fazG8SuB%2B1Arh9cYq%2FwMIBhAAGgw2Mzc0MjMxODM4MDUiDBQFIKQ06QxYFHTzHircA0261kYKW%2FVx5HvSddeGLu%2BHRXQRzPGgVeS6TbSIUmVSf12UklR2o9uMnGw3hNAi5z8w3g65HDFWmqbXWQh%2FbzdW4POH9R1dErXk9m9dFySLci7qTj5RoPUtO7KEQtyHqGvtXW%2BFzGYvnvLHLAH4vlpoSNwUYPry4yDFzYfe5XCrjrYDodYhBYm2988w0YuxD3vzAHWcOET9tr4qx5J0QXStzTiB9hgBOeCDEX2nd9L33j3tj9f7jMlu1NpQk%2FU%2FbBHppXfYC7eZWWiZ%2BklGa72ueik9iAgZ3uV%2FYTOV7hk3QVTJjXORRRxN0Z55BHaSZ%2BKn9AYwIEQ44MF%2Fy4YmmgXxvllpnbthHh9h23KUIk%2BO9SV9h4%2Bz9De0xa5xqfFIilePoaSMQbb3ZOaNAGhDJEr2tBSGi1uPsvcHsFrbFqKJaF9LyD%2F1X8V6PfCY6C68f3PnG0XKF8qYkHwGdD5%2B8Sc5TOZONGSIkGJyORTY59Uh%2BPg7MrsyrfRuftq2%2FmtO9AZQsDigYCLTTO4BmM0l9J0LK8u9amE6Wd%2B9RHhreTcuCbF825hbp8tCYdylUNXgT0p7Y115M6JwUG5OL52kW%2BN9Sopkq1EP6tklzGBd63j3obrPY2q9Rb4PUIluML22x8gGOqUBxlWv%2Bojxl5fwBNHSvqAIJUzzQm0zPpzpZwt5JfaxkeIrNKJnQqeSYCcX7Y76SXSUEbUbBHt3D6bdKlQsUNc7ZM8K3DaiK36tK7xUJ56lH%2FdTEdAcBCEObQsPaPAKa%2BaY0M%2FlJ9TLuYzPmIGWap3o9hTam1HuhZ03BPO8WVbrNEkSe2uBBXZjE8LDox4%2FwFhYdShK57yUTT0BX37iNp6X%2F3GDN%2Bwg&X-Amz-Signature=43a70eecd938d5c218e3a3a51c3785c07d6cf24add8301695d371aedaf0a11aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

