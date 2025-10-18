---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RSITP3OY%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJHMEUCIBNSlvk09gDKwJTWTIV1LcvyUsw4CNjlzc419nYYKXNrAiEAmVYBSWTKMUoLAbKvAmQAZuYbs3GS52GP0KJ4jLSQG0UqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHLZMB%2F%2Fj%2FLLYcanMSrcA7FXdNCTajlLyARQpIO5vTfI6eFmLC2wOcDfpCpuWZRKSaKsV14XAL63mTXYM%2FX5XGj3AgeFvIY%2B5W8qXFWRw9YvyWczS%2BTCFXQb0bCz9kyxzJrgShMxcIyAnpx0gSdIK0jvtIbQEWUrCoerzqA06JEdBMxWLgwFHmIU0zjSBuoHtVffLdYl6J2ntfl%2BBpKMd9MTuii73e1N%2BRLLdftgXMuSRrTsAaZ0aBZRggJu2e7%2BBNzm4dMjJcHkN3dv%2B5eGzGTKzVW7Mn6XZ4BpR9B710fD1r6s4xH1R9EvQA7WAYHzewd%2BIcUfjzhGdkHMvoCDpP87%2FAq9qdNfJuUIvIDEO8B5iLmxQTYa5KJIMABttIt5LetCcfqTgeESBYPg%2FTBg%2ByK2lbbFsHVTomjLYJklGl0T%2BdNx63BdRFzLFiuiZdiZ%2FoJlIw5X%2BgXSHiCc4iFgoRr6SNmEeUcx%2BSuBmU64GJpazBv%2FM%2FYgJ36JxnnnLLm9P64vTld8PFNJnUaSpiM2jg5P5ehOo945rGcS1bkAPRA%2BpyUubcy6xTQIXVr3Gbn0Spht1EkVcBtbgtSbpRO2oLydTN3Q0cBQ6liHlCmEX65Z%2FFgvzUzHdLAxF5V9%2BZ0wqTANx7KI%2FOsF7k7QMLSCzMcGOqUBT%2Bn0uOC7F7FbEXV3ob4TJAFx54XZ1ey0WzlyCLhhzqDdEfhJ%2FjUEcG83AlDHi2Ne%2F2ParijW5hwKppiQAFqLXSpHL05Dw%2Bm2JqbC8UIVX8NvEWTkEcrNEVRq36B1Vv0qSBcTkUfwuV1mfNwGcwn0iPcxJPp%2BUJV5VUJSCK%2BfYlhQkBFbDjIB77jC%2FKPpSjrc0CBKZ%2FzYpRf1rbOs3P88eupzF%2Bga&X-Amz-Signature=257649ae90d9f3a73dce3b64c72c1372bfdf6aecf388412ab358fc77f6f63bde&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

