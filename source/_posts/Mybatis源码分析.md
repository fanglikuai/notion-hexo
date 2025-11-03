---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TD5WR6KY%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHHxy1PzUqS%2F4hBNvSpSdpUMR7N3USlT73ieAM%2FAD4uvAiEA465k3sNB6wxOaCSbn3l0TGpaPbei688FlDAZrJPyoTIq%2FwMIYxAAGgw2Mzc0MjMxODM4MDUiDFBByUxTnZHdsHC5OCrcA0ltRfosXiNirPzSWDu%2B%2Ba8vCxQVe7LPfVko7OWw9uQSyXCfZdXvv%2BxeRuByStPlhfiuOAFaTvcDQqxDqc%2FRk1O%2FLs7HsZmpXV2%2Br1glopqpgfyTK6Sru%2B85Y%2FvGoIcXNc6g6fAO6%2B6S5EsElDEH7n7dn7KupcDmgo%2BAJqwiFtqnRllGwUK6EmHth70118yeODcVc0dYmlarw7qE7UVNhXiMkZnOhlFvQPgzfma5yhliv6hVg2L3iZC8F%2FxAg7s10oqWUdf6rWMtlxoV0rxozjZ4jaYr3pYT2OvUtr18Rh8K5iM9xsyVwBz8uAvCSgWK3BBRu0eo12kGikdt9aIKveMk7CAeXuPn9q6F2MiQ6rZTOrWfzwSQJtalh9ZpezneA3MbTfijs0OmZ%2BXmoEEFnkTUmYWKhZEV6VaJhmxyXcD2AYW%2BSq1ioeeyjPmvd%2BDcMBdJrmkfDMPc4BwRC02CmSaXVVPVL7HlGxE5TxVpByWEEkbIKLZmV13DA6tupjhq0QbaV7df1xqc4NDaFjQSWMdU5EU7eZ1x6oi0QRl7Y%2FRiTQcDJDY729AIRgPO%2Bt3Iwzq8tsymkztOWcMywzEFHtSpu6JcQ8swrq60eK0klC3oE39yQyfjZmidtz3fMMnQo8gGOqUBiUpSomUNnK1Z%2Fp7HzATSEUl9mK4JisKmgK%2FJSTRqIQm1GricSKOmgoqmloecpT0fQIfACZF%2BZ1tKJUee5qlWAcrUH0Sx6TpK9fy%2BWsCtjhlEwVA3qcBFH2jieM2TNld147ulOkiqau4g%2BmuICnEJVYoIhSXEqh6Oa%2BVxWNGRptLvIP7%2BYR7bFIcAYOlJ4qWvArbnsjV%2FLPW7jmB2HFbyihnmGm45&X-Amz-Signature=873d72e51171ffe84799e5de0bbbb19d819817162d11f97fd0e1640c639b324e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

