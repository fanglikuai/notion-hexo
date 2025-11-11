---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6CDOKGW%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIBdZ516OGwQlrKOQfOztAXzvWvWy6SxGeXcELGIuGNqKAiEAnQbDsGPC0SGVUqnyqnZPQuFAT1ITTIU7%2FsVGCcIFy5cq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDC3CgI7IfnOWDH%2BrMircA%2B2wtjV9AaLxiSZZI9IvGxv4J8M%2FBoLYSFTqRDNjkSyp8vyCOFmm19csCF1jzZtNiIT72lPLo7QP%2BnH2tC%2FqxZPoxRsCSObkMTpc2UT2CsUSKPUO77MKNSgmmSZM%2BTMnVwYGTG8FFyHpQU1w1BdmRmEBV%2BNfWC36RWQnXme7g9hr7GQvbyuVY1P7VDPb7oVE76FluA%2F757biOeOJbAusFjcBdmuQGBMUrehbayyIIS6RaoBeN%2BhQYv7zY8eFgobmJ5NZZZ%2Fjd0aSg4tSQaPk6Ac8d16cJjZuSo0dJvHuByVZObUSOUmvJwkoLbfa7SfdNymr%2FYm%2BbdBrO2h5qx46p6Y2Q%2FQOL31zjcAuX2m4Nh2BJF9N9LGUQRa9ue%2B6RMVsyDTkrkVCqGfmVCmbkZfFSoZFIcXoZuoW5kZo%2BnltUY6vrlpIkuz2xDpBhyBSHd73NVVKfwtSeP9eL9Ddqu0l2MwY6qK9xjfceHRpHsPfIOOkOBfmJZsn7PqFXaZwHRMqTd%2FGBpvcXgPiSKp0E%2FHZCPtOgQw4bwkE3qgyIjYczRpOstc1iXQY6SZYbqmXvGsC1ME%2B0xiXqqm4cqewiViS4f2NjbtEerc%2Fd%2F5RwmWEzkEKCtsWRlsAEkMSbMNzMMiozsgGOqUBUPpZAucFW9ic8eoVubdVUzN3q5HXzIb6vrj6nr1cyX7HdRG%2Fa6ezP%2BjcvOalqZOiQVxaAwAFS0mOaxBSXGsG%2FjKtV5EETHLyYI%2F3rntA6Gl%2BgcraUtmIvl6bT%2BStEOrfniavW6O%2FjzwjHmFcizSIKCX3EhsWloNk1QlvxzZjRZFH21ShUzetW%2F3AJr4Wdj02JkvOC0ehHmDbcsDaKljebZ8IGCpH&X-Amz-Signature=d4e4fe09d65ae643a70da983d8c682b295712b258313ce6ef2d64484be7901f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

