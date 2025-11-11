---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDHL7R33%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJGMEQCIHuagCeGVPJmkqJ92hEnHVli3LVGAjiKlyCAxnsRK3OYAiBVSX7l%2B9KkmRiSfaDlWcr8g0DC5zEWxwYkpwWSlOHH5Sr%2FAwggEAAaDDYzNzQyMzE4MzgwNSIMBxELaOmc1HkMjA1VKtwDJRh6RT2fRRBBF0sHBUEE1RdZly66aHwm8KLU2gOMNsmYOzlG1zepbNCHq7qTE5h5ACfxUvCBs2%2FLJ%2BE7gluOfkI4lmnVtfFfEGi9pb73k8SUHrnQZGvJGEKhUIoH8vTbVx3awr4Xnd%2F81m9sQ1OXhfmjdUo2D38ximW5xboYCECO3tDCP2hmyl0UM9mx3wmSS8uqdxpnaQ6KvI077iRIvaN1WVxMNB9sh4W7IUF1O%2BeSt8jRUz7zBKbhTMrxanhwuCXgXQkbkcKmc06V9Y3HNhdOFSz8YVgRtf7QvfM5Vlp0SOxj5R8xjKo2ap3oz3Is2VMD%2B8CFYX7cj%2FLls1EXflKA0S%2FIgztuefKeYmSRQu5idwQgszmJm%2Br8Dpbw6G2wnx7w6lQA53B0RZnlLPadwevfs3%2B0vF%2BO2%2BmzDjb%2BFP12T%2Fy829zfyiQLznR0pZ%2FQ43PeSawhkkvd8qp4FG3sJIUkVbEe2mR3qVW%2FSlpW0IaueCuFUo2vxISies4%2FSO750js854X%2BujJCCuNVm3yyRYNcCIXRiHJHNF032KY49aieEVKkis3K5JIw5p69SEK%2BqfRWLt8A%2BHxD1Siv0sxIqF%2BQlvE8jMog7s9ov1uW6RcaXcgmknyMNM%2BUmmkw16jNyAY6pgF68rHCAjAWpUjnn8z6YVx4a%2FGVhOuwS9jQlh3ws%2Bcmy6MdXBrwD9tPYv6f%2F3%2BtIiCB1wmvEzNQQVgMJEz%2Bd7t9g%2BsH1yI0Cf7kddBrDrMcxC17WIgx8mM7rwqw2aE8o46pM%2Fv%2BxxVN6tdcJZQ9fHFfJkSIM9Eqb7VkJ9VazdMyV6Q9KJDbJOBqPWIUO027fDn4BE0hFf4zqibS5vSJS4ZGe5z6zZO5&X-Amz-Signature=2d114b9df1bb76bf871389463a92dcf69d320c765293ec311a46ded84077080d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

