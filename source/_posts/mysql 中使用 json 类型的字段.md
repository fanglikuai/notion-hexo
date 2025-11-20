---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U2L2APNS%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIG6AonfCpgAyXFRdbSl1Dr2z5Bf7GwQY3Hus41MVPzpUAiBTaK0sUlbEUeS7inUBZXZJ319aHZb8bvETOUaYZLsdrSqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwl2mxYyPxuMNERXBKtwDvsZa6sQZGlFECGeoZ0pNBgcjWTXoUzm128akmxRPKKMGPbQOfN%2BJ5%2FkgUSUmzpBGgnzF%2FzLE%2Fw5RhnfEs5cbkC3XQhH%2BzUuOE9UohXZnHXoeA%2FF5U9LZe8n4gOuWa78zC6EYYknbR7qz2lHqkNjGhir0x8S5j3QjJunqRzJ%2BTrexVe6E6ecmeaVEHiBCG7tJ6Jbp8QI8B5dD6vHswbVcSgGBCvHO9CEK8wLEZBIMm76gU6CZrNOm2QhLQIdOMbXgTT5yZSqNn9iM6oAruD5xj2rqckKMZ3aoR3531HFYLFB2yvyaUXaixt687%2F3H1pb%2B7HM21yvPvMx7ClRPm6M4p%2BbRVTuupf8mDIhlVv1bN2XIoLOa%2BoHznTqOf82aew8XhMTVSzU4ZwP17pNkloWx2BKJrYAomxHt4khHbq%2FG8%2BXwpAbRIx2QmN7xr0Z4wxY%2FQzJpwOALbYVF6JHRt%2Bsk2ERgo6btKMs0JKVkmAqZbpHpwrXou2ckZvhH64LJZDEWHa8%2F1ZSMhO7%2F%2BfAB65Q5PCJwYVgiGbBkHj1%2BNBOSpAoEPBqvAz%2F7y6VUg7sfjmzJMqC0k4lm%2BkD3R%2Bh%2BwnoZ50NUehoE4n8UF%2B0O0TYgaawQdTteFx2Bn%2FasjLMw8MT9yAY6pgHKy7FgOUVb7u9lZmENKWSVS6DUghfpoSE5LtXd%2BUdqlpD93FFaTSaWw0iIropWrT0wZIvpruUoOsysQHfcU3WUqa2MzvCSGYYukxR9R5a3s10%2BCyQr21jGByaUYp9OntnKNKmbsAPNm3Zw02HBp17W37O1mfUy4m89I9cVSKJ0KbM8piZcUc5UYwmFk2uQV3CPf%2BUcd8PnS96l9G5jQyvMtb5MFYST&X-Amz-Signature=438535a50330c6324ff2613ec50553edda691ada7e2642ac47dcab5c2464dcf2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

