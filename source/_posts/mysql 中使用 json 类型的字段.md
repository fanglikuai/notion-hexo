---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46654FFNMPZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcowijKpCxXGn1inqZgfGJ%2BGKjMPk%2F2TuJADjB1anX7wIgdLul6ZpcoMtxH1f6vy3X3JpoheIVbDHWZqjrwl7PnEMqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBN7sWTscKE70L5A%2ByrcA46A3MAIuon8PIuXDOdzOl8gQ6KuxD8i5c0UeKCxP%2B2qWsvIleuBzO%2BwoMhNJOKr0u3p%2B7FRpoF40MuQf11FRjR1Lib2mtTQ2NJ9q6ubyNtxwW0ZeB6T9zY8BUW9cBLwMZCxFDL1h2N5ayLbi57ITnBbwDHplvxyP6muNURTilZLmGDemSgsdk0lqxR9jXZGsfZhRmHMeJJ25Y5vVH8kab2nFI39AoVlPb37S3it2VBwpkuYcHmen1s26TovrVAxsodhgyRZt941zwvPCeBwq%2BT5VWTEZJKg%2BgCocvm3jiNw%2Fvqp29eJ7oKNvI%2B4%2Bgb7zE1ARXDHs9AOF8JkjujDB6DKEwWGGZGqqDFcxTQ%2BpG4b47jjT0p63yGhWOInCX6Ki7ooSzQt0zG7T%2BUMrIKgHF7KEirIR59uAHyT2201ZWJT46XDHMquHgREj4I44%2FhaWfafi6FOdAZCYNM%2BaNomahWvmHWnd2iAjjkNZPnZdHjyEhfnMAX0HAweXrnCeGRnjU%2B%2F5i5wF73pdjiwTU17%2BUdB1aMUVtObGkYHY%2BkwOeed3YBM2HieE1H0El4%2FcWx3fvqgIW7MziGoRAIWfIAkoZ6OFQg9h86BesRBJU9pJcZuip50Fhx%2BVIQ4JHheMKKg4sgGOqUBierQjN01km92dsFSFwWOX49d0Yzb0aPYuiyRRdt9u%2FlNr8zoEGJTlH4ljC3UG6Xj9B1g4WSWGUDRCa2ODXsMmaQfXdOddsx5CN2uYptgI%2BixsJ1WJb6XDtH%2F5UlsXks0hkkTvIn6S9TtfjBkjoPAqj4DwkCk7nt3qdPzPsQXaEKm06%2Ff28q6qSs2%2BYGzyph9V9rWOgMv2vaTVFHjE6ij4kAikpJN&X-Amz-Signature=b36f48553b4fac0c6567726250a646c39d8b5158e36001d5bd73a1c2234c912f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

