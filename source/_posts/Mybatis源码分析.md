---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46653WPTHCA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAoiRfPmElrUXaIb5YWQEenxG4e2Ss2anpt6SrvbrZujAiBOaInzAqVtCJEbdMEcJcLCevh6xGYPxgCKhit4MdAhpCr%2FAwhVEAAaDDYzNzQyMzE4MzgwNSIMoiSiu6a0FeF4RTKLKtwDj0RQyO1LCw%2FPMX6m93qPVu51AByG1Ml3CWph3ft5XFGB2vTmie7w7jaqQXVQqCddgXQ%2FYl%2FHSdxFRiahugosXgOSmZawyghZmGK0Bh10fGnZw6TNqPDhcZZxUMZMmGIbAv8MPgiD70UCFFJn%2F5hoC%2BIUiwnZLITrvqr1gd8NCVA09S1TF5Mz%2Fey9kThEEFCv34GSHB4nsi6xETeG7YZ%2FpoUVHQH2EwahdIsQ5f8euZJzpi5n%2BK3y4Ax%2B8yOtIYIoLOX6XKfTYKqJ1g8nlk1h2f4%2BqPVudyw%2FafyQGX7HVFcBYfg0%2Fz7ykg5qKIcp7IUMMJGrph78KKlTXyJnDbU6yt7G7lQyXPYYSel%2F9mUx5QVnLhScF2n1LkEMgL%2BM6qDLlD%2BW7CiN8lLwM2aFtZe%2BIECgIRwvQ362pUYpwdBnx%2BQys1QZCfyIsBSza9dHIhlbI%2BWqfxYT87DBryLNWcox4cZXFcwYgBrTMWVrc9V08HprPx8vnnkehoLF%2BdpofAzyuxgB8RnSLWUnbV1%2BvCtNZRGNzCSknWzG2w0IiqCa2um51wzZ0ARBL5lCSDgU7xe40T%2FMadhB370rAVBY0Th81IjVfL4af1ejn4Gj%2FNMLO2e3S9CEm4vg%2F5%2FffVUwvb%2BCxwY6pgGaUwMrCQ3qJA2hsCY0LTO5IjQQwgYx34b6fc%2FiW7sv%2BvDYQ1Fq4DdE5b44Rs5R%2BFoeSSkkxBX4rduUMv7U2krhgA5t66ua4wOBEm5ou%2BwgYjtePBcZadexUnpk7SsfW%2FmHFobroPsjeGEuXXibtIvzhaNlbzzMwEv2BRn8d2kEzB%2F%2Br0iAjaCBc6VgPNQnsA%2FU4Thm2Qgui7my0YIBJqskt0PwxnSU&X-Amz-Signature=5441582fd8402baa2a44bf48b2f0f3013cadb59ec98ff858cf99c8e88c32f59e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

