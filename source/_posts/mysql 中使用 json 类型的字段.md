---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABGOBTP%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJGMEQCID16CfREx8QZ4mOQVnZUYKqtKKOD4xn%2BmjbLBERRQR%2F9AiAY2bSNhnQc1jU90jPi2CLexvFq7bQjfENsTlgJb36L9yr%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM6yngeFXmbCCoyFDDKtwDYJX6wHgOvqgDdZE46LR9JrPmjfN4PN8rqLeRwzPv290o9Dy6y4Iv9xqf%2FgVX0kJaPNZ6RXt4R%2F6kpCfsJV2kOSNs2B6hDnbfqgRM9BY3dNbHCR1J6ZirWiUhQdQmCQFeQKs%2BUi6RYWiwfJHU%2FGnBTaR0aElz%2BLgJL69ceS9HVoiDhyu0eD6UDz4oRYD%2BrhSJsSZvfo61hI0%2Bj%2B0H60F7YhDQkEZvl4G6PmvIdU1wVxu8KtN%2BeVpOpz2AZhGSMl1niqfG9hl0z%2FgYUy3Ke3bEGPjUdejmJWt23ByEUALHf4CwIZE%2BPApNX%2F4CjJQSDBXdx7HKI73o21fc4qh7Cu3QC5HhNEmsNb7r0Ky6bJvqt%2BFGBTsaqa9CHEDwLSmiODiI2GFh5hfCV%2FJCWH87FlDlc%2FhX0gPbALo8OoLqob5wWNSocAifUZ30QIswkhHHcdwEM9yfsyhWv9jHa4luM5hl6fqFQqakd0SuO%2BRx%2F7pN8vr%2FuUVHcX0FAF1OD7Xr0pCKk87144wQhLA8J1XIS%2BEg%2FNkmdkpaRyDb7j95uf%2BbipLslwP5ebtZUH%2BNedY0Qe%2FettTYD9lM25isgjY23PSDda%2BPwWtnkdRGvf9kCiabhOpJVsVVxIuqfcfSPc0wnOL0xgY6pgHzRikk9vpaajYs0yqTYqZFnzmz4HhWK32lAdd%2FT9sr3h7GqtCbBUhBelm9h1%2B8ohM%2BX%2BZAl7UuJhfKjnRAbpk9WL5cfbDVEMNlDxhIK1lJeXzUcVWqyb9qw88QSuAc2KA7ZQfSsWcIKjM2cIw%2FY6y3adykUSueNoGVG9brArrp2KRHQtn0VUpCFv9kr%2Fc1H7c5vYcArqi7dVIs47Tf%2BQoGJHMSFy48&X-Amz-Signature=b1ae372e5cbe14a15854cbe9feb1f5185aebee9bdc3cbb2e26efeb28dd64a1c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

