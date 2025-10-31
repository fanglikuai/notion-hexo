---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624I733DT%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJGMEQCIG1WjY1ggjmWxjVYbvpxYhorg5FAY0A82KOCHxb2Q%2B7GAiBfwHwB5u%2FpHmU4ybFqpkjCoeKjYrQKqq92xGUlzg1PgiqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZWHlf7lD2wwbmVHHKtwD5LPIO6CpA2jyxbOvmwIJR4%2BVtik0k2iP3cDrBO9YCmHutBRirDuVl3%2B22lnYTeXDSVERW2y%2FATCsvqY3S7U8JOZztz23IlQeoWpcCbmrNCnokqvUxbmpL%2Fkw3j9VydyXrPwB6U6KmbpMtZwTPB7tz%2BkIeFrCHQtbOk3Mn23%2FVou756ArY36wzoxfY4NtYHHXzY77dbUepD0GvF%2FyeqmIvf8ILR6AX85vhVeaveIONs4AZom6y7dFGmbSqZxrOsANLKnZrpf2Knshn9xG6v6NqJskyOcbAJmqSJ9L45gO8EBpLehpZnkx%2FBDhQi%2B8xGwPErcyDN4atM1VdJgO2AZQH2oO6sEa06nf80zOoghNjzmCdMEVhHqIuss4Bc2p3UK7UhwsJPRbR3mx%2FZ68kpemPwkORopEnBfz%2B03L0izrvHYnMSg2%2BvAyrq2tLvCurWDXzVCYZM6eqLUSBmchuKa56RdOPEyZmOI0vB7zqqkcoVx2E3LDzSBxuf02oVd5M6YfbY0BY7Gud%2BjUBpkF04Fo8E%2FPKiT6JzEJpvAALMQl2GuBskTWCqwbV%2Bn4K6odwF5qVeBzPI1zAO0bS6v%2BM69P1reaX8b4N7axWVEAX0hxGTupdGkszJYs4w%2FXZhMw58iQyAY6pgHI6JyEbUW5Hw0gq9nTSwgg8ajWhLlTtzbgZajKZxXSOXXeJ83nya46mmU%2FkEOZ%2BxnpeA%2FRqQeVYeIbCyI6mL978XNmHAP0J4ZBe8ZGvoGZp1k2oTi2H6JYXY%2Fegwt7LCCMcQSkdS4w41u1c4Mxy2GdjOLJSOtBaPc86TzSYLQH7DgnieNJQtY3C1NU69vILp2hJMmAH43RnluGPxVFopzW1b9%2FwB9D&X-Amz-Signature=e0c7d66ef0cba49a0cf0843e0350a3e863b511dd0a701803adcd76bed9894be0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

