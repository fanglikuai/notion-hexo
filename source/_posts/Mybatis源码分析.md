---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RISG4RN%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDnC6oKVRwGHXfZ1kyS3kF2%2FZGG%2BMPpAEC0UECo074TpgIgMbdMbubjeUGiLCSCnotxVj%2F8OhbOG2G7cu9OxxCy3EIqiAQIuf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPAoUgOvfcwYYvFoIyrcA%2BXUSYzEPPxuVRcMWhxXX%2BPWZ4sqB2SYv7Bjh%2FwGItJjq%2FU7PjhFdLDEg8my1DCIK62MpurHG36Il2tlvLLdHH2Gyvk47UitfzoQPwRl8cHjdtmXcaa13Z7OtaVWNcfWiwaODCvNWv%2B36Li7nw2EXiUMp2D6%2FA0vcdJ8v%2BGiASLIMDGvgWk1UmF6nE1OueTP5zDVfLoN%2FkupDmVNBxe0%2B7ufWqwZd8dSFuk2Q9gGqX5mXBD%2B56CkJFO3jOdPp5o2wJaniofKLzsKDqn%2FbC1j5mqMkIO0skRC1rBV%2FWwBipi1DEYv9szPUea%2BOC%2Ftq7BFKzD6AyTLP%2FYKLfwmiHmDjtWOeipsW18dImRxrBSO%2FXJV0IehUw%2BIxNk7mHcHNZIGkKZOo3IdJ1Bj4qdJwg49JIu%2FIlJp5sS9O7D4QrSmiNqckf3f7CrlSIbqp8yJ2mXcyjZhAzChcWi1JLm3qVdNDg%2BD4u01hdEhRgkTyqstV6wHh6icZHhROhy803T0UbPnb77uXFU%2B79sYwQP0G9LsCXvI1S0fR%2Bpfyd%2BPShJvPDIkC3TeoUp1NgpYntUZqgXlbDBNjJTIVA%2FvY%2B8n50k0nrZJuMUsaxNz4NbhEYAugK%2FxZuIligoJx2Ixsix%2FMLbjgcgGOqUBdevUeGpkCfyN1tB%2BqUSgLdMRSaHovdXcHHgwYvhcZOyn%2BUdN24Pg8d7aJ8CvRLAvK6TZGwRYwZt28tmjRCJmBuVtYWOazMpt2UH6fRUel6tuPC%2BxVBfhwi8OTXHXgK9GRczFMLUGsUaBAanksTjyVxAvms85o%2BkNCjjK84Px1R8HpYyGfNHx7IHNoK%2FPXOEi0ZHI7KKuvRhad2%2BVDj%2FH7XcAtYmN&X-Amz-Signature=269f7dd7c60ce65c87d0d515c9fdf448968dba2632d6fcbf04668e7f599cc3f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

