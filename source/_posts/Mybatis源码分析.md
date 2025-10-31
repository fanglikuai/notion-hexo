---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLQVZC7M%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIGlzKYSK18sxrb8prIOqOeVKbfd0945MtTSdLaciarhwAiEAgtSdI1JPGrc1hRHqmXkeHHM2SecHWVDSo5wzqyJFtQEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDEijubBrhoKyZvuPvCrcAx86DBTGN52H78M4D0e%2F5yOvTXpzmWbWZBbrXEbhTePhWsLMfcGyZc4rV1G67ScnAXCkTaEXHD%2Fv%2B%2BxP1PWBQBKou3Uo6wh37h9OuMthuN%2BIYIo53s07ZNl%2BQkTX06QSyviwlaI36wKTolyrI9TesAqdpvIXSOFO9UJSMeK3fQ2NuyOpq%2B8EAyh2LpvjuWh6mB%2FiazlShk%2ByHUmyO42KzbjAM7cnXHWV%2FvXhHhVwd8YnGeauwsObtq7raY1J0cnDctgGvvENjLT5tDOYWyPJTGatZVXWlCDuf8CW7sC5ZQoVVTTz1iTXlDK7CmToG3aIv1w4v6ndvW3HReGd7roccteo0KodnBh%2B58hPgtuKQP1D%2B0%2BQh9oke6icTeFeh7194jKhTR8KCpOfAgIVw0XXspKnOztvwWp8WuaqjdxaCgB5N4JBh3geEzwx0bAIIwdlY436H6LJFKzoES%2BTxQugGzQh8qRS5O5pRPVfImDxBsi0SM1LnkQFDWhnl7nKVscdr5cqOvKypRoBm%2BqKjFa1sJPWkzKynY6yY3fWIHSrBiPs4cIS7RIMe1Ji%2BQOxv9FrodNu2HqTGi7DwUIF%2BmBUUnuoLucgbd1eI8MEVx2%2B7gggQoDDyqqVcIRSc6d6MNaSksgGOqUBcaWN%2FdAhGQSLAbfgDFeHk13FADnRG3NYQswkgyOZzh%2Bhcy1GwKEF6yHCn0Yx3t9xDNcvIx7tF2aWnoLzgQAZnv0of5it33O80Dxf0njOk1Q0DPSpZ57WrMY6O3lFK%2FZrbrYUGBHVGS2f1SWnC69WqTMybZTUziCb%2B09Qw9RT5rPAjNPEvyUx5typBJzS1jHN277Aq9pU%2BGeFf1LZYqy8ge%2Bgox3b&X-Amz-Signature=a3642e4636d705166f4455cdc902fb0ab9ff860d88be0fd0e255258d41723ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

