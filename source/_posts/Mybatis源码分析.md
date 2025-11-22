---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKMMSS3P%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCpNmgI13ZSb0c6ymxng%2F53cKWj9CgUuhvrIbFzvHtqXwIgY36DZLOEHD3ExO6CJ8HvOANbg4d6ySa%2FbIVcIrdRn1sq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA%2BL0Wdyb9WT%2BNUAIyrcA%2FtP%2FqcvgVVmnmaMLymjgD0yRcz2z98V3uEx2HSFXXi6obd6HoKR4fgV1nlC4yeYqilKEy6Ruj0JCGcReEUI8CAqSA7EmxIVppGLvESmZ5W9Gcm76sT5JAHFcQMuTYH301LBXAsTUPMxVem3ek5XXttm3psO8ghbHfJFiZ0woEYYYYS6ZaL51lS5YDEh4QtU8R7FDrG2hTx919w%2FICIQ%2FgX4qpPhova8UgoDtR2sk3EchUA%2BfXHmWr%2FOm%2FQrQZQTY7EsW5aPbcXARQz5XjPOooJNMpTkXEc9cZ76eIN%2FevKhV%2B7X1xpaV9QJoh51GSkirpfb9dpkUu3BuxMAxmT6g2%2FFuo1kvnbQioYiVz7kAOC845oGjZ8T%2BivlPEyj8%2FAECR6mlgFZ0kDKgbmCPvp0gn%2FnO7w0Vi6JpvoUYaXj6Ntge1tQWeffWA3JeflVrnXWDMVZkRxrc85O9Vayop4slpFZt%2Bh0D2LcAuXzSrW3rLlPw00BogBLcl4LUBGuovtY57BG7pqm5j9MKzSSSnIk57EyM12xQ%2BnBXZe5yET4PEMvkuD0z8%2BddN83%2FUTrYPFOQ%2FQ%2BjeEb2NpYyqRh%2F12xQlcNNFf1EJUdk6FsUEhFnPhQY%2FlUamb%2Bn9OXRVRWMOjkh8kGOqUBxd2i0VUZF4et0zKulYPPkQql7l3GcsJ%2BaClnObdhDMEekXT%2F4%2FG5i7PRE3mUibWtRShPPLgehsRR38OILykpr6vROZoUob8LlMKto4fWiR6y%2B0xNwGPCl%2F96C9S9ZDUsEmVkpst67ff02o8BhFW5%2FMZXiJOppK3fuU04griYGb3J3VvrY66J8ePL0TTdp9tJh3QCrFlNKBJ3csr6eNNGuArUmE%2F0&X-Amz-Signature=0d4cf7aefaa433b5c3a6269e80a3bb7d72f766b8f66ed0686cc8857a16d94d54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

