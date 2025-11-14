---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667JEW2566%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T080045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCG%2FKagh2jm%2F%2FookKHaXPTyMDWodSj9tP%2FYbQ%2BDnKvYJAIhAORH%2FlSGN6KRw6GHWuoafpRdCCe%2BrqqYJ72Ap0Pqcr6gKv8DCGEQABoMNjM3NDIzMTgzODA1Igwv5hpx91cshR1cZZoq3APo7pnBXZe3HQzpbUFs8y4mIso9ISqgq44vz%2F2UuXL1jD1tvtCYTVXVdVbT83gB%2FR5Aufc0wKQGM42rizov5iRWfvlh2uYZPrqDgQfVgZtppsQF9qDBDw44ZQDKxX9uCDmqwB2lPRSJJo4nnFTaXypFo0GLG0HTV8Ih3cGPaTPAh1pp3Evp%2FPManqlWh4Zc8d9A0jYxDKIXDUFcGs7LsnIaaz0L6g9c0zXU1n39zFYUVvztJ8oDIcZsaipZB2O32VI64Di8ArTtsmBb1qgmZFUEhYoyhOxo%2FCxoUKnOJNRs1NYE6BNGwygt1Br%2BKAq4CGRJpKTEwyCwbI%2B19d2Xuq%2FLMiYcUL6dN0v1k0hrt3Air%2FqFv4OPJ2XSe7VpQLhuUUkvlnzI3AYPflkmzPaiyXyG8sKigMFL6OkSCo8hFlxdAFQxu326aMHnd0MraIkHWkCwOS2RMx7ayONkuK3UvdpZliDXts%2BHbRws6tBkLG2zTe2TC9DEIeDyMi8%2BTLV%2FzuTAfH1iZwHFWai%2BVoNyvGL%2BIdBWQJQjAfFpqj%2B1K6bUrylayWmDQ7Es730aU5nIGdGKP10woG5gGGOdrU6eRU3z8P9VDJ0Uo3XphjtNV09kOhRByul8FqjxyQcfTDCIwdvIBjqkAc2uoER%2FEJd49sDb7zJiIFWktxYfgLMJXf1YwcuDTWeFGUFAJN2CiPNrEwKYLsRxkzqPqTQySLJ8cPCSNf1fWSmNAneyTAI%2BIbTrIIaPg4J%2FLuQyiO3vzE6sfsQ9CRl24m8s9K54K8QZT4SJ%2FTkzWiaOd6BIc%2BiwISx9r8wuODuuH0BdwnksHg%2BXQ2iK6QpByYNOVT%2FS9chAQIB%2F7Xx0ptsq1dt7&X-Amz-Signature=9f9bcdf900a422e26bfba182518f30809e6eb4d4a2ecb2c3fef4217747b49b84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

