---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XWJ2PBY5%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3mLR8c8IawyNKB3LBmUAbO2Wbpyfw6erWmDTwbqcWKgIgFGmtaVnrDxM%2FiLIKNj0jS73qchlwBo8ZnhdQnXvB7fQq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDNOHEmOrV2u4J9An%2BSrcA8QrB5J6c4Axh7w8z7cwty2P4q6jpLEHum6RjQtpX29F3jkn4e57tX418BHfZdoscgnMtdUYZMWuyjjQ1qDKFWvc334PyjQWQ00gIi2I3%2Ffm87O7QQjz2RndT%2B10qtd7YYjMXmvdGcnNU0sYBJVywxXhGrt1j6LPdpbiEQqhCH1iPpIcrn9c6DpJFfEgO90tOwfd%2B8inD4ilUSpcyUznE%2BcuCenbfv%2FPJVwKk8OKuXX6vAgAjON3Htl%2FA2blO6GLSuU0penDLG5O9K0dpF4MRWB3AODmEJgpDc0Feqh9MajobuHZ%2BJhuvKi3bPYhbc0gTtz4wIPk7Xq1%2B24KsxLeAgIwKRVgEoWGn%2Bv4jUseZ3BLS5FM2Zq8Q%2BGLdij%2F460P0hfhiLaYETqQLnnMsJp5bgupYkUWhhvDg%2FTJXRhbnwOHk6Gh9GMWIhwk7RPoqhtqAo4KMK01Zky%2Fuo2UI3qpiFNmeFQ5gk%2BhGgW3G4nMsKbOxzpCXjTUYsYGbqO2GJxKXjFQX5vj%2B8PrZFBQFG50BkCpriQKjQMU9fV7QNjtJh9stpeirST%2FmxkOA7lyA6I2LKdYQ4XgLGcCxIsM7gwzmNGLx7BR6adndUDyt5qV%2Fj47wClnS7FghDo8wTETMOH11sYGOqUBdlCYJ7Bve%2B3GmIlNLfcWRvwdMSGZRC3cNxQ9f4zGPd4E1bSppvEE9ysDR6V1UGoF%2BNnAt3wd8n4TjnT%2FeXZXnuvT2gK23cNKLjF9WTykPn02XEh9nmCPjneA9s7yc6dtaDLgb37gBHDO%2Fng7ZLf4s6ERWo9%2B4EbjtCmDSmk2Nc%2BSDm3CKQQdUXSVbEpPigIjp3U03Fh4%2FU12ZLdlQsdC2r%2FOdMyO&X-Amz-Signature=e832d5c9578888a6fa676295d0373bca3d403fd21b6a0246e586647849c443f5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

