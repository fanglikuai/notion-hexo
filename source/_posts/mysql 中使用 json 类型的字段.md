---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCLKWJWB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T010054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDj7mFiNCdL1yEoSikNhLhF2G0YmOuEDcCDxoluihWQbAiBKCIjY25oZUdhOoA8E8Ecq%2FprykYh6v1gmmfUzcPwNFCqIBAiC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjJGlAb%2BDcFEzEtarKtwDNwIsCO%2FMP5T9jcMQBbCINPTMpcnzLPybTV21C3i7wN7QtLLlX%2Bwf%2FhbAQGEBQT%2FGCFp%2Fe1co3IrGRiEK%2F3Kt2hLNLgNylHY5CdGUgh%2BlLQzi8vMQJT6yij47enYWETYgg1%2F3n5bLhJdZGIaD9%2FEn9qb5JuT%2FwpkrXFprfGk1VMmEIAfXrOwfgFzEZQWT60s2BJeKTw2MAO480nmojxKBiIcuH%2Bucn8XcRTsSjep83PyT7%2BzMsTZNVfBDmY%2FiM6%2BTsgZoMdAy%2BuABdBYahHCOQmw9y%2BiHuQa3ckUHmDDgwusqKeAVUhaeF0Lts1mYz8mfHtfMGACrRVM3i3uhFIECu4yQjSdh7%2F2qAf3l7gx3M2770KgxJPLZpW48yAp7CMWi%2FxLEhJyXJ7OGCiI9Fq4m0%2BumofWQqF9XBXa8TopOfX8Au8i3%2F7xfQWyrV7gYRJ2ZKoRRjKLCjlpIwA1oMA3e5%2BW877FSGsDuBh%2FIHWbHZaywKpRxJyWYSQBGAbHOsLRo7patT0SnqcgzemuMOBjOj5lbKjtanQmWvZuhDyddJWLgCGcQl4IKND0xYgOotm5x4GJu06WlrtsXLXjeKeVwyNemcWyxYxX3io%2F0uqj1GT6PIb5qefVp9VECr5kwwvrAxwY6pgHOE7czl7wIUbsX1CAHxJP2Nh44KQXD9loeHd%2FJFiEtNZ5zbSLxq1iHstxV0Ejsd%2FVgAKsYTlNPRf8B%2Fk%2FV2eDQhJINMkoo1Le8FP%2BOC%2FbPpNmJV7MUg2SddzKzlW7MEwfYAbZbvZ0SVgjS7DWir80fr%2FMmv1QCE6zujQX%2FiWbcv47LBlRsB9gLDN15Mu881xrcc8XSyEZZ3mj8s0WOIJGpqiu6UZ5J&X-Amz-Signature=6f69229d306b1fd5ba9d88cfcf4f8d54dd5a5e0e678dbd8c6bd3fa0aa5ac98eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

