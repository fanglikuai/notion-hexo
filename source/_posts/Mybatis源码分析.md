---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AUCAKC2%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T130053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQDfCDLG1leuukqA1mCQbRejXYhl1aicE%2F8dm9GwA8DLgAIgZNG4FjLIB9KqJKi7y6G5mp0LoZc3TZ45tH2mgC4WtSUq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLJImjtO3A3wJyaecCrcA9O442gtV%2BZNtwE6cWzAF6iiSV78%2F18mbUceEnRV9muw5eqNLgaaNqpMpy2S%2FZF4TbmnYCqQ1YtOwoTgW0IaCTijHIK5A2G1TXXXdzViHFG5OKPTIU8GnLAe9vgmqxweWjFN4k4xnBgfO0oE8WNe6FC%2B5FGNoDAbtZUupmbvxig55gRXeVcw4HpW%2FIrS8tEwAZYwhWGeEdnMKsj%2F6aE0doc31VgBKXwhtrE0FSS7iDRK8Y7y0EEq0kcQh8MWTWUprajZb2Hn0jcNkMD8pcBhYtlX5qqzC6CipbPjxWqLHIpL66CvXQJ%2F5tjJhMkVCB7lMfn1DSMHJ6o%2FcstK5%2BgDkbhT5HZYUDXwKqgBrj60tPXaMbccm0NgjYD2ZoagTKmhvfaF1eahJFviy2xkCegx%2BXsioZX3vw1F5no7G2yCZdDNbSC%2B9ryKwnjSgPj%2FgSSPmGhxHqw84U6Bj%2FIBAtAbXAdjeR6MMP1LWYR3DvsI%2FHxMtQSa2rBqWSwsD4cuBPcQuWNhASsEY5fHOidHIG%2FP%2FWgtOhoMvpJbsYebnwO4p841zqIuflqqXoAwx7d7f9tAtfHEH8kH0UAR%2BhSLpxlSL1UDSUSJRTfemQqhlI%2Buf4njPb7Gb4NBkrArBjaFMI7XzMgGOqUBYHUQU9zIVe6UJp%2BQ6MG2qqs2GqUVm0Gq54ewKc66pFTR%2B7SUeSZPyfIl4sfVJyonWOCj7m7WHYS8p8KLBImmY8QhIHmJ2Bzhh%2FngUU%2FxewY6eSLP%2BoTgbjOLeTZR8Bq2ev%2FKfbdCnKzG4hZ3LlRKbIcv41iEJkmHL%2FBYMWrse0osdPh%2F2zLx%2BLHYAexPgySvH5xp%2FpUXwoZsKSA6%2BpuMY7Pg8gnq&X-Amz-Signature=80a3f73664c21f4aae843feb07fe841433e543711d37ebadd4b57555e54fcf2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

