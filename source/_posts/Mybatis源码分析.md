---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662O7TMRVB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQD8Yo5hH%2FtL%2Ffuc%2F21HRv0jGUm%2Fuk1XwydeRtZNR9%2FGkAIgNZl%2Bfe65qMcZALZlwfJpQw8DiMgNYQbXjyrZcCH0eokqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLNIlAigwRzllmlaYircA3xa8bP2cW12bBmzrHJC6vz3xabu1l0YEs4vimeogBpFhaaJVDe7qh46wPodXha9vjrjdtSbHCVtLr%2BJJELawkcBlDXoTYhC8yQyPgoa4STObhcKtpwNrPeauSJlayYGPTFHhomK2WOGIjCTyS9KPAHyiqx8%2Bhs53qVXpbCYoflUlS3F3gTZ6F0I6EX%2Ff7CcU0no44iLrcbKWvw0g4q0UtxU%2BbZXP9F9qxDsTU7L%2F%2BbFn2Gv%2FkBBpEh5w6MIleqRgP2g5hC9OJkXbIDtSb6DQaUstB9Ze6ooJ%2B6%2F49nNQFTFSiyC6ZNa21%2BaBvnWtUEOYvcYKC14WKR1GjVmWBje9KOf2M8mAeMulwMXaFX7LrClbJVfSdz0X%2FiVaTzPmU45XIyv1XcSad%2BxVrya%2BS7qwJbkkRiIHLDBIp8uRC651GVAxywQDakNm%2F0cawBQKZ%2BpIxA1yu%2B7AN7beO%2F%2FRyAnGf7RsAyS7r9DcnsdU%2B%2BQb%2BWq3b0jXK%2F4olzUegw%2B8zko4AYommtZ3ZEZv%2Fa2D7hMJT%2F2nifZBzWAP1PfBCGatXavpeNWruEm8MA8RepARL4fK6qPAGRF%2BIp0YVfYmwjPZJjKcNCowzGUWKmyLCnyFK%2F3L38imgds3ePbOylgMK7oh8gGOqUBJrm1F2ME%2Bd9v9%2B4QIk%2BF3POTziMpyU1JkJ33GisfdF6x140gmRVVWXpeZQbvMKvCE6qneXekWy%2Fb39HEIwmjW0Ndxw1RB4VKESE09bIAZSD5wkdrtb%2BOpENohZJybe3%2FWl4%2FJPIFLDOW29s%2BDT%2FNckGk%2Fp7rPjrT9nvrsRzQ%2FCEGerC03A3ksas4otFHUKJC9f%2FnRYczmrLT3YLCGtxnNYc9Va8M&X-Amz-Signature=cef6270a450011ab4d738779bd49890d7c41204e22bc00ea6625db0800c6c018&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

