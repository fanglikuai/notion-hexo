---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V46OXVUA%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQC9RjdydfdzBdIu%2B%2FnK4CRmoC6SgbvVadDfOywE%2Bx00YAIgY%2FB%2FuKOol8h3RdOJdo6NgTRNdV5Ztt4y7d4awnbv5Acq%2FwMIChAAGgw2Mzc0MjMxODM4MDUiDNRFDuCaOQcE2OBBgSrcA1JW5wk3e3CFbnhP31ppD96v0yPyf47zZX%2Bg8rKjAJpqWBpHakZO%2BECbarA%2FjYbvIKBUrrF8Eii8gk6%2FbiyWkP%2FR8lXLK%2BPPgMMv%2FO4wM6mA40%2Bz1LfZE5NaFsR6pxH2%2Fvu1aJZ0hCHZiiPkxbz%2FLYLCdyFDRh2AqBejiLU0qmk0T5WNOFqWx8vAptFgYVfEJ5LpL9sohS1q4UfKskrDz7O4%2FKGEDlqyObh5AWf7D%2FqMTNl7Tsb6gsREI4DcsthWMvTaSb5A4rCbqoVOeVxgLsJ4MB39PyMLaWgSig9qBN4wOGBBUlRBP8Re1I807C9Vrbt%2FFdwtiitYc6spf%2BFJwI0f4LVu7MeJGmkUd0zpWuJr93wJ6JKlKIJ89VrsSKUccNyfhlGIBoA4qww%2F8YVyAhW2E3GHpzi7I08lRhpQ6JnxKqbqpBLVphMu2I3%2B%2BvyheCH5y%2F0gYYB%2FC2BVVMlqLquACCBNw6H7BkR3H2Uns%2F%2FsP0g6oGIVrNP2ujXSelacvT37i7j3KorTYoiCeZAzdBYG%2BGzWYLQeB58hdMijUUD30n0H4%2FKOVQGv4v7YTGbQ9m9GRCtYizbBrA6ku3cBCEjSuiomfQfq0dcePyFaWWY%2BQ7GdWAb4Epx%2FlJOTMK%2BryMgGOqUBOtSocY5tqQEw5u84JOJhRtQhsuB5Dmq5w%2B6MP%2BmF6HbvMw%2FQ%2BgJSqosaN6p5IF45fPPQMivpvv5YEuQjbhkxEC6uOfuBtg6ee68Yo%2Bd9EYM9hRK83noNn56kqTzfxJfCOWCByRy%2FuBWBTH%2BvHIV3sjdudv9Vmd5WiKi0NqtK%2FMOwEkS3RyHLF7g%2BgE6fIS3fSOOwPffnD%2BbvznjABgJHiQEwLFxW&X-Amz-Signature=48e6573bfcbbfe303a5d623c737767428bf756a656a7dc9e4544d05a3667ea03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

