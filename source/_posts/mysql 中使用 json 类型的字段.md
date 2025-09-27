---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZWDUYYV%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQDmsOu4ZireEJP%2FFAwvQmAWJhnTLYS3vVjWrGhhurb1kwIhAOme7wMopVOwJLQAk5r3moILh3km3cH1NeKOhp0XtmJZKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwdFuUq%2F15hYkGs3iAq3AOrQw19xB7E1p2qaW%2FIzC5sYE8HhjkHkn260BwPjY4ZzqbFLBJZZZ41AUsOX9Tny%2FPf3ZsE6iTjVgLPLAKimpauOXh9QoXkoJdwEuNQt0YJ32xCOGXmt7TCaxpF1fHvopcoMsNfVhX3wie9kKln2VkzT1IK%2BWK5FKjLSv5waSQhOJdmc%2BOZ7TO4IZiZNCU8%2FMGgT4JOYlafJFoHwTSCc6HS%2F5OnRO3psbhPfWMSblJPLK%2BiKBr0c%2FgmevANa10%2FdKSZS6KZ0u2%2BwawSMefZOO6A8z80%2BvrEYdkh42WSzlZz1QaAFMPr5QDu7EPODxJOeMQ%2FXBFhfxxju6L6F1GTd8usukp9JZujqSZ96VIry0kuoJ2JlfF1zM6t12LLOisltTItzOimE%2FYLFHXHzmPtZp8BmMZkg3OKvlgW7czMgC4b4P6jWHNQXDNex4koBaSy88n1mDXkNByu2EI8atztFl8Fg5NcTl916DiNIUlW19MIO%2BI4YZ7D0dpJW%2FzMJXyklbO%2B4a2K30p2azBB4Wit0nEBMsmKI937Cfikgeks9FpNxFGcFylTm5Czak%2FPgePkTCYDfQEs7Wazmr6HAugxNyagHHfWZXLLiyN%2BdqgDAQSTwNrKa5Txfn7jOG3y1zCs4t7GBjqkAaueWHz%2FpdLhIAObKdT7swPQcBZcmIgW44WzHy654%2BhEjxAy2t9gvUvB49vWPCu9lHMEiTzGytBy%2FWX3uwNtl4UOXtg%2F5LQe53i4sRn7gPpAnEw23Rde7r5gJ8dnXlnQV8GTon4WkZ%2ByMRvAJ7shik0qDS8ps7BUEG9w1dkdI1fzQPgLi1ZYNd1T%2BbbUzNz9lrm5Eogh1X7I8%2FJTnZtbHaStq8Vs&X-Amz-Signature=1bc39a9d7f8c9c39dcba48a6618ff1c5de6fc9ed286e9b5c2a83efebc17571b8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

