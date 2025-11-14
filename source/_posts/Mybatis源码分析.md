---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667OHLHPLZ%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIANR9QDL4dBUGvlk4utpFxAI3hzAIVALJRZbc186S6N7AiEA3%2BwSBVniUqjKPd9N2bCtQL3WYpR25wX9rId47dunMLUq%2FwMIZRAAGgw2Mzc0MjMxODM4MDUiDChhenBz%2BIaHXeUzBSrcA4tASmYOCPjwm%2BiFvuMPqUC6rCF7HaqFp6f1lLbq4j1yCk7GkkX0VOBN6lL2GOG3NRkoB1%2FF6qj6ZkES0F464PeuvfAHLCaHGFIARCXk6QWVdVTUqSgwpPiNEenB%2B5TOqLgnfKv%2FJylhfoFaWFQQr9EeRmdlNWf8cH1XatMZDhCZrE8Ilm9%2FweMij%2FIzQ9%2Fo%2BjJsIA9K%2B5eUndKs4FN7C7E7Iy1xGP%2B5FOuB9BE3%2BiYeFPNQPW%2FYJTpU%2FLkeZF8eHXelwOQHD%2BTzVzCz9PvdhcXBCn0Ek6xkkBsLfM4Zvidf1emlNbqEjHoSwcNIt%2B33PiaqCa%2Fq%2Ba73N9SSdi%2Fw51idMYwDQGZpmgmV1XWoycbMGAlFhWEBXXogJQ%2BVO%2F3o52WxSMG1%2Fx1T%2FdbGKhLVj9hyIjA2hirFQDQf9NB2NJZnpfcraUOnTxrvj85bFchNV6%2FvsUesTK33PKqG2eAtZHtdh8p%2BLBPOr6rugHntTwYr94Ed890538QsTvmJ7jpIx0Bw9ADbGhxKlWrF2CtscZGYxSyXBr2r0GL5nhdgh1iFt%2FMkCoxiM%2F24HZTHVmMHoM0gCqPrVoEw9S9bfDp82MeG6ZqIslaaDK6s7WUz96kr5fRK2o4Q%2Bw3s80P5MPC93MgGOqUBM6HWhsCERQaYzgKSAeT7wznKsFlXQi4yhwpVYpgba7LeMGXN%2FBx7ZsAOO6jDagc0Zw%2FUI0PwImOC%2BIoi6c5JGHT9hDinIFuZCi%2BhroKofXJzbi1iyoAKPjYRqvMEYou60bbPdthF31iSPj6epnRsEkkHsB1lXfFEFG5bP5Ufrl2oYIy1VrCzT2UcbcPirH4fUfQY52xAYkTFBspcBGnzgnXyxZDf&X-Amz-Signature=06b4b2d0d270a52b9aaf89b009ebbcdafbcfd2a67a62dd954ecaa4dd96f34e4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

