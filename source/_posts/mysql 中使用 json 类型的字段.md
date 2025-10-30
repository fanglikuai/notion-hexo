---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RPDSA7JO%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T070056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJIMEYCIQD643%2B5fQQ%2FZoJpPXRUI9%2BQ69RFyD4iXCDecFBnwOgDOQIhALS5s%2B6GF7dx3KM%2BMNO2mT%2BuoRSCGwjOptLe%2F%2BJilEocKogECOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz%2B6lq76TANv%2Fkz0Moq3AP4e5L3woeMIlO7wVozVB4HMrgFbZyje3XHkpQqRrpXtMlK%2FbM8JiP1ACUuOFe%2BhqOu%2BhS78xSHA8vtj%2Fb3zEykNhWf4e8UN2opFD1z3jBLz6uXIDUZtnubWvjXQjKsZ%2Fqxm9vWFBydAcS7b5iO44OqSfg6oTmWQrBkp2i7yngLBQDEL7njfl0s96%2FLhkjb18rxgVa%2Bg42Yz17emrlx3Q9yKXU4wxrGMr8P7Qw2U%2F4Xj1vsCIlsjMaCKzhyhJPkvblMspyBWwCNq6hih%2B1VUbvsyICVjAyN1bTQPFkOgbFdS0ALnr%2FrbpRCErTIGNHd0BPPN9cNT%2BqaCLyUXp7Merc74GUFJitSi0EM%2BW9WC6mf%2Bq1%2BWfP4%2BERxlsB%2FqwnxqPfuVQeteHxjKi6dNvwg17lz9WfAKnXX5lCsfIRX4WEReXSpH%2BqIcSnNSNkDrINrOSwBKwSZY0dU3qC29lGJmgkaTq%2FMYjRiFTTUzYlI1VolBdow0i1IS%2B%2F%2FxTxbxlJ545Ikgf59QhiGgiuXaqyjTT%2FwsUNYehCLmlpXbnPC2ZmYPyS7QJsWUCut0gaYjpZpiByBlvkjQx6x0iIZdx83lVU1q60FXTe1dwB%2Fk3y5GAepirFLG9dETD3HPoF9kDDslIzIBjqkAVSjSXEH%2Bua3g7YvAbr2POoeJ6OOYk2SHhQqpkGoXrZBj7lMTsWTj%2FX3uOdl93U89G4VPxkEnUF04lMWeePFuSzmTeRG6r%2BKXS1lThT%2F2xhs1C%2BwTQQ7IGz5UzOu9HnLQwurKO3RdBP3jKXQhDuQ%2F9Xdg2EEEB7V3wONnnsQtGMjowGiiF7sXO0cAwIlQ857a290CnUL%2FJVMVrWCo9StfmgL%2BYOp&X-Amz-Signature=50c4b13628b1e1841f65abe895bcb49fa94058ff8bfc131765523fdd9ffe3e4f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

