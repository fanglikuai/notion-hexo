---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THJZCJO5%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T170055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChdtgX4iZHi2PQHRAzLCfOPjEfWjTBSB4rZ0E7pyVS1QIgFs1Y4DWso%2FXZ5qGWRsOOnHW8nyDars7822D3%2B19MPtUq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDJNSmDbWjvgAGJKTWyrcAy6RljDyOSd7XTL0TMBpsVQrPJ0uoQVJa4LjjNe36BPelNVuKk%2BgjhYmvLm3k4rqkENU%2Bqri5j1SgLi%2F6kdp9W2Xi6UgHQn%2F0cmCPDPeJ1QLNvQ3JgcurXJ9lW8lZf4PfOYL5fwikAnlKwh7ksBhoH7u2PX1yQqMbiVONYltVxJoG4sbtwcvmzjDLY7OVjCpr5Lq8wL9zCSzyksF%2BnkVQ6h6uPY745zlvpDk5NGuUM5wmy%2BmEEelNnxppENlRrGXlcQVbST%2BkfM1o%2F8Fbtk0yahGtYaDdO6uFca%2FhDeucrZo36hnzKCTBBtNMSe8d9L2U6zIXJVOGsVMaianBgeQ7Fr63YorrFZLHv8BJwYT4VWR6NnzGCxZI6HvRLS4Ptnm5nObHK0F2wVncF%2BeaUf2efs4zsVZPMtnW3t8JGiFufPq85IpK4DL9bOSSXiij2QR%2FyZMgW9ODWDqKJhnzvw5hFxFUIhUQpGj4b2EWOcemJwgvEyCQ338C%2FkCQ5sajfVlVMjrLhHa%2F8%2B1cVeGZrW7%2BSXtokFvkQ01w%2Fp1greysCbg0J6%2BFVlXO5%2BOKaJRHYFnKiyIzXRgE9Mf3aXjJ0IZCpdL9v79rtTmJjcWZ3KJoT0J%2FZ8rtgXgi5OSGjJvMODN7scGOqUBMj0ObujOWuTDTphYbUpz9Xbju1ByKi0wThG2PT06guArWgBB0bSFPD5EG0QPxgCvqEQ5pVoWnu4htJmmtaH%2FRmehJmCvCYxfZ7od2VBIRtTgLYdxUonFGvreR0PxqjcQfqbS7rWeoLTf8jFOJN8nmL0WEWDmD04hb7tcWp3XzEqdlndBZSmlnBBMRY%2Bhts6tSL14LnxiVVw8mVmpAFRYFK6jh4L4&X-Amz-Signature=84d9e10673e67c2f685882f8bc579ac553a47d8c97f1e32b5f67e29350e61890&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

