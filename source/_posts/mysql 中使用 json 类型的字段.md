---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKTR7F6W%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHAaCXVzLXdlc3QtMiJIMEYCIQCK1F8i7WPteYiDI6H%2FaIzYN58lf4DIQHFueLv62hTQRAIhAM7IyAFSsMxezgnsUQLEpltc%2Bwlhb49l4M%2BPJU%2Bnf2SYKv8DCDkQABoMNjM3NDIzMTgzODA1IgxG6ASkocpbXHCM8o4q3AMsVX8JuZxuh6C%2FxkL957ONwbfplOsC4mNfxNO3e0f9NyCGoFwp5%2B8J33Je%2BeliRELLw3hmH8XVq18YcMe3VowOMh9rYbGzK6CNjov5UK%2FcJxk62cj4%2F6HgJ1FAdaIwWeVB0qT5slphjKhy68ev1J2XGhxl6bTDbBovewdRP7yuPikGTEaZMbQ3IHM98K6%2FObOFwyyGef5jyet%2FBiDqddswd36W0k38UQ5wUKYU0lu1INAS61drSaKBmvPe2T7tTPamvZx07AvYrWHrnNhq%2FNftCtWMM3UzXCcS8UoTre9OWrftFgi%2BgPbXOSUd0dS4wR6VnVZomC%2FMC0pvWkZ7GOZTOg79YO6SsmHmqJhZLIBOApke32C9vvrun%2BYZ71gl2eCLCLGwvJBYYcdeO0ggaAystd62liTQoCrTPWrPiVsT2sfY2BL5jYQb2enqP4v6x5BIDq7RIQlf4BOTOL6h40ZDi0BDBMwgUla9Jbzm7JBNzabv4Ys6IGwtWdIuF13OGQud9lIKk8UnFaSkaKg3ekEMwrGt9e2ZRiTlfYz3FLf2OhgimoY9ygGb7Jr96emgBwd9sjCuS8TMJYhLsLXLxmvcJLDoUW3foVwH4ilUjFRLoMIJi3xVqkl6UOPKPDCc3NLIBjqkAZ2yGNc17XUo%2F%2FNLRNLQ503leI3S%2Bao1bWKd7a9FEWgyVWO4F6rNj6QxLK2vwdjZ2KbeL5RiVjDprRXV3AOxM61hKoV9xOY9hzKgxAhl3vAo2GyPWoF2EZXyuG9B3%2BFF4QP5s%2FFSW7iYLqc%2FTlr0ab06H7EF7GUZENf%2FN5t0wW%2BpOWWNVAqtgGQfCfFaeMQ8BHDqnY0iwE8CynySrtYxsZgegv4V&X-Amz-Signature=922fb9396825915f55a5a61b5c04b88a658c74c4f2c6c9b46e21474261fcf838&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

