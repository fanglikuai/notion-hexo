---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKIGQ2GB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEKR%2BZbxdrFhOhFuhC3EQ5B%2FWUWnq1b0TKxWGV%2FeZc2WAiBHkuB0goF0tIFtESyhFzF1YLkcV%2BN3CO9t9zYGB8ZO7Cr%2FAwguEAAaDDYzNzQyMzE4MzgwNSIMKpYhLcS8Cn6aFNA8KtwDrD9Kv1rvgTH%2B2bShWGiCqt%2Ff4IRFlvTQMAH4CNBqEONhlmwKVin2cfVU7PNtjSAFLIbT0nxvQrSNWayfqT91TrgiEiwiDNhmo5e2%2BPFcApBXX7AJ1KTtbaHkiuiWdHNBJIWGzb%2B9Gk2ih1pNrnTZE%2BfIxBrE8CVHnuY2HSh54BtnipLcGcKdQk9GqY91oB%2B%2BDMDp%2FwtuUkCdvy%2Fkh1VJjtIfOZXv3%2F5e75nIo3PYKM6Amcv7bEK9S323spOatZZ%2BxVBsqaxUx20oROB6XUzoOmKdMyujDiE1a7lOr6d8WxsyJW5iNdadFonzvC%2FWPIDFL12PZOlbvK1kySTMHpX7OJjePRHLMNCfVRQ58tASEbfdw0E0%2FdJ4o0%2FT%2F4hOR6qpgv8K9DuWZ5kH013wXTJAuydyuh24T5oA%2Fr%2BKqy09zW%2BDnlZ1jsxNqVK7U5WoECaf8MAp8A2EAe2zezgLlYpNPDOFzVt0nvc0Qf1E5uhEGYyxOlIRMCUFcqoV4gbO01N7ecCHgtIyK0AfngYbKr590ONWWuAJaNG0qIAqyIqaaaBxULfaNEfmptuRF1MgVATX6e9rOvV15QsfX2Ka5gsb%2FlYxGjVyDvtI6lJEPilzEjSWGNqaEPd1%2BvDpmegwj43FxgY6pgE35fUxhqBX%2BzITySTORJR8IGd7SLKdAeHqnP68m4eZRq3ARqXA1BC698Qr9lqKYliF2paQRteuXVSJ231zgygn7JvUwmHxuWp25eRgKtQDy4aM8SHJUotphnCngVkwrasY6zXjGN8zHlhdFCs3SWnULdxmtU1F4hAgiPV7Yu%2BzJz4f45jlcd8%2B0PyQy1e6nxG7VpJpemKFnb69l9NhN18WleFFjprs&X-Amz-Signature=ae13992ed9874429d402f1f421341dd71ca2402b805ed13ef888d100988339c8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

