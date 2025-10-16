---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662FFICMOW%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6bSwJMr93rBSr32EMDyDQ%2F%2BMbVeiGlUQgAvLIF1I8zgIgPyWZf1rzBgP5ypyPzeGRwtWSdh2ooPoYpe0oPZ9bx7cqiAQIhP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJh84FUlx5TyDbojUCrcA%2Fk8X4pMsXs7H9HJ5Wfdu7WIze6w4esOzHX8JIWUte9WZ%2F9Pv1FZF3CGNXo6KecyOwhhzdRTEp3w6K1xc8a56uYWJz8LBmgAkMBzTmE86uGDAdH7K6smku6CvDQxFVtlljJDzfwaUyFKVs9UFE4j%2F6N7NsE0ZaXssB0XMmhxDQEST2zzqtUgABJVH73ByerG92CzSe7WxYZOLltFcGLS7W9y7I4n2GUHVH4Sair59vAwo4BrzT0ahXtgjTCHu2Ur9fjZicv5ouvDRt4Sa517Un57DgO%2BEvmG%2BKW%2FJF8ZLzSXSUEyiatRczUoMLRtPx2je%2FKgvq2rXXvFyfPyPhOWo6kfZ1Dm8laSyt4s7NnaIt%2F1d8wkKHYJVn2KnjlAdkNW8kxrnsQSJ7NXsbVSBIN833J2k%2B%2BE0pOE6a0m0kU1amTwzkTAkRwYEd1nW54tgr7IRtd02xtXTuuhqjHVjGFrrrOfhkgADyfMtcPA63Le9WZI7Vx9%2BBEp4EWZd1sr3Y77zJrLUdIgYZujfPg%2Fhb1VyDB87x7W86PRAaDgeEPpEp%2BR7ExPP5rRd1Nom9TiMIkmXpUhMi2Fr3Vlmm74dCn58gPE9l952660DBLTZzXhe5UIOg%2FOCLfxTM7mKrG1MMHAwccGOqUBZ4C8XHLMAkKTC7dFoZiCgGTc9MfDzvb%2BTLsNIGlCotLZbBv%2BCAd01Dyr%2BKQEYtzNVdkhtbMcl9URsUFBUBGMrKg01MdAY09V8sFQf4O8%2F8q9NGhJhbnjOS%2FzoCOknoMF2M926Dejk4naDtqWWwYoBKoRE2VbfnXHH1jga%2BNy16sCXMYUSwi%2FZ3iwc7KiW97Q%2BVF%2F8gLfEkCV%2Fl%2FAz9MofYgLzqbX&X-Amz-Signature=b4d4f1878fdf0a24fabbada6e9fdb37a3e9b90fbb75127bb2aa416d5d5dba77c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

