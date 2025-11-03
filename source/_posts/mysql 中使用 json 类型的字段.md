---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U5I6ZIXD%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDkADk0KKqvs2wnToCi%2FKm%2FRpSJfkXpgdZn8xsQunOzpQIhAKaQ0JWncJiu%2BAliPLcxMK1CMkM%2BahtFwlIfsOK7O%2F9AKv8DCGUQABoMNjM3NDIzMTgzODA1IgyZUHXeSgt1UgCmigkq3AOgOKuN5%2BhFJ0GXJISXQYfYnlvKGV0WIUIAlVpR8Df7Ld5dn0U%2BfuHkv8G%2BhE4QMjF72OPx2Yl6Xr4RJP7FaiIz0cIrYtcWZfYyUWfK7RX7gs%2FWPrvwmghm%2Fry8qVAf1b8xcJ7xR9h%2FT8E3u2n5aUUy5c0raKjU0J8CGckUPsNmnQAJjN7b%2BTTfoQAb%2BGREXE5iCxQlDI01NfVUP2UkAXFvSLMZVreyUKCSzrwODDULOS8mMNnyvJNYp6VOcHson2K2TCVfWvfluCAeNhclW50nyn0xvKM7gGIh0trBQXc%2F4N3CET9R4vh6w6VGB5QXpxn5EdRgcvI3r9AkZf6w0RSfHk8DtaNPTLihAFqQ2xZhtOHg%2BLinrwHPNkK760KRNynz00YqhridXbTBAvZq%2FqqEPV90gG4Q5tAY%2Bvt%2Br8qlr7K9RKs%2B0xNAWuD%2BDp3EFwFHCvmp8aGd59NRSVGxoHj27RJYZ3uzPYTHuBPXfijGpy7p28o2%2BiITeOI%2BtQofcganiV3AMZ9c7uYKh8GlCEcVKG7CRkbP5sYFG6jccGyu%2B5V8x4xWeif6prtse%2BoSAOuJ6NjOvPKBZZIrZOXE4BdlXojAvEYIWksz07kt9R969e0cxkxqxNb%2BckpdIDCEk6TIBjqkAcKGOMv7rjMrewlFmA2rqzSi7zqNSavZ92OmPJ%2FWKN%2BwFOqzdPGVKlgAlCU1Pta55gyQGPsCyVajc%2FK07ihUyPrbr0jjzEz9LfQ9Yxu5TNSx1I2iJz8avoltb8esgSVTCBRlFDcp2n2j%2Fc%2BKrDqLwXzMs%2F0mJeZqlUSA9cpYTKtKbswaV6hsPVnsTvpg1H2bupPT2OEHM8JHfEX7oAAI8oxxf4q2&X-Amz-Signature=ca2d320926b29c055ae19a7e3c53fa32bedd0ec68ea4a8f31762cbea4d6f4f6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

