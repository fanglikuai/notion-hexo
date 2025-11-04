---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUC4Q2G%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdAwdk0ur3TAc5S3uatFCVWbeXRijCCb8Ze5fOkzJXGAiBxC798hBCDuScChU%2BudC6IYhQvxbfEG5NrSwybvGTx1Cr%2FAwhzEAAaDDYzNzQyMzE4MzgwNSIMs1RdGeblQ6gll1xjKtwDCqFF8aSD1t7Yy3G4i8G4RQoBA4IiHPdYBHBSwn49Vl2bNoBMGr9mXHYAn0VnziF0BhtEuCXrZyT8A4J3f6if5T394iMkMDLAPN17FsZ4qBDirb%2Bqn%2BbN1NGszkf3CObx8PVXI9SRoyqc1Qdrye1oWpYwrgrAm7DjvntrvdC6Bnp%2Btjme8pYa87IqbbkW2p0aMDYZJ%2FzjNLeh4fRKXi4ogxeZoQ2bDh4p4w66fGdtkrrhtGb4W%2FF1GPQxwgOATCk3I4BwNtbFKo77fQ0iWGDr%2BfwZCOobRUCdIJPLrhjamv0NmiAeqsQ3JlcSLRLO1M1g3mvAvix%2FeBxNFMx4NeRhJI6ff%2Bb7LcG9Xetk7XuVMYQ74AF6menHOluB6dIy1tsaofZsuVbMszD2Q932vyXhj4GqmFLcEfO2TbriLUJcquNsk2NoiLTu0RVZPc7QnMx%2FfZW4qRno6LoGntezSVs5dcvXSd%2FXUNZr43Eh2eyjOvwiIpc8VOPBhCICp4vyM5SmAbFdmW0%2FKXH8Oo%2BC%2Fli%2BlQFNcfoj8lhWqkiDZTVKVH5pZ0v%2F4A9nk2DeUOpUAnekVFftzwuWYYpwdr8%2FOg9O1mL%2Fx6YzXP%2FDQdQZKO3mKB202CfWSyHiu4ahbAsw5pSnyAY6pgEnEcOydm8fYrF9xomqXCh3z7XRtLFyLqE%2FjbgZ%2FTbWXuHI4KgJb0itl5fcPFu6MnLo8AqkkMP9uT6r2zFOoCi3B6yX30vVEeyJmkc8alHJTc4imWPPG8n8ZaaNAI6k5djiPxCQb3EOJ91XejsWpK4N%2BxqUwGGMcXWLJRzO2vbIIhIrPSnZopv7%2Be%2FwlG5mQ930QKhH5lLAeY1sYTdZsvW0saVPi4Ua&X-Amz-Signature=c54f0ac879dcd5a186d673f15ffc5f52d8e9a14e6d48b2aab5b958f185d0bb64&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

