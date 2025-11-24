---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666IOYNLGR%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T150158Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjE8U29xiu7mfofam%2Bpnx9G2NdV7VBapyZQUd3zgggSQIhAPy4wKCqt9wjS%2BgZgzpCvBWkQcDCrGZvuQRX1gdtDOy2Kv8DCFgQABoMNjM3NDIzMTgzODA1Igx0fQkhRQa0W7TEzpEq3AMC5znRTxAmttIh8Q%2BX57d8GqjCoKw1GvFhKUFRzkad70NjsiPCzfGtBWz8OG5ZIAQnRNvklpxTEIP13UgOdQJTxmqK1hX3yYRkB9dFrZm4aJ3xfTEotuK4BMuwAZaHek6D248%2FVIKcHc12drDW3EskxzWUFECsQh8gh%2F9ZEoqdQpbD5Dyhtle5A65yDIgOod65t3WfiO9B3Zr8hiqiJUNC8Io%2F8341LIFZ7GOyszEtz2VaWzLobkmuUqxvN3dSzCZfgprrnrmhycD%2B53Owq8VKzmWfK1CTPqIKOqVXLPTkZjsiG5oakBhm8zYB7QT3EUpJHDtKHZ%2F4aPp2l0QNK4XYxkbRdUnM4HxS91t%2FEZorU6T7Dd6vSwerMupOTD7OfCqhDKXES9GRnpEHmRz6tpwlJ1ZQXYf6opiAgd6LaaiOu2G09OiI946HfPCJEGMExSJvAeTTIpyNublASVTqFLY2UYOyLXXiVvlPYovxk4HVfUArqXMiWXululRp47zD9QybhmWwYRLX7G0wS59EHhLV0vAo7HnIgOCXTH%2BKvDdN31PtsHCkwWC2hzOYkIQQ%2F80TXl17iTprIoBkRj8XD9tY%2FCk6uUXdnzylPxgNw9kC0%2FVhcbbvX3qeqL01iTCa2pHJBjqkAVQrkFX7n5XBezHMweEDlJ4W0QQjY1uT98eZdCTuLO3cpfRTtmm3MVbumPfnzQhveCX50tpM8Ajpdt3gln7MBI84vOpgTDwZ8dbvIyXxdkqe7RWc0t59vAxJQ6qoyBUTL6a2izeQ3E43bARbl5Txzno%2FlG91b0QnfVdmijn5vqAui5D3cowHTpkgQIzuPZ8vnTWabAL2MaVK1mefD8ZQijkQG9a0&X-Amz-Signature=fb90db2311fdb1b86038220a5dc6cf77bc493ec0166dd893bf14dfc04c548e27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

