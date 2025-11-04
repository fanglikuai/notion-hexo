---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PASSHC2%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGaL12%2FQ0Eepd0nNK8FsxAiBJTRWPZ6JEfxHmRqP1aT6AiEA6XVUlbHym0vwXxDBXisSuJFYHnp2WS1SNopw1WNuywAq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDKCsujg035IWROuSiSrcAwr12%2FaeVx%2FaR%2BEp1xuSdctW3vej7pwX72O3bXkaPqzhvHAXpnB3e05berOftSzqP9AiLbigB67x8DV%2BVg198XBtWIks%2BwkKhR2QpxRJuf0ZV7MawdP5XM9P9qdkHNOmEc26L%2FAtfYkQOW0zPFSwmMSRL57jGVDmLn1Im4wbSsP5dtH8eZCZqObhvLGE0u8uLIdWT%2F3e7suDjquT7GiWVGJctO1xWKy5UDBBLr%2F%2Fy6PmK7ufZsr31izJYkjtri2CkVw0BEoBn3fUBMw5y5lfI75pQ1T15NpkHrF0D7LMBJRSKeJm2EnyVFfrAwUshqRGSDeFgb%2B0mOsXmmf0pBxtbIc9EX0qr3PgdIMTaNkyNyxy6IccigNYaKCaHEMZnRv68tGmKs6LJsPujOipu5xKeVkeLL9sGdzI8uSYKz59Uq%2BH5uILkKcLJ6mcUT%2F8UTsdm2Cihb0r8a%2F%2BGLx6qtvLU%2FtSadXvKuV8ZC1BJKflFFKM1Wakvd2vFUWAsW0fFNzFCN4hy6b5qwgR7Gtf%2BiMTZ8%2FkaoZBlZIZn%2BrkZaOthnBkZvhnKTOj6qX43s%2FtfUL2XYuLEL1QIjpE1XItywqkIkMGzF6mmI9hsAhgqOv84n0KMa85dbs1zn%2F3HherMMTCpcgGOqUB8KtVEr9j%2BivoXzhlfDrPJumTbSRSxpMPtpQRrRfrszxDVGDucr0kDKPqY%2FLoLBTR49pKR7Ilm68MR5xQ%2BueJqk7czJQC4XahS8BfRn9i7DNcTN36eUaBBfJeCVZo810ay0JG4u%2Bg7pGwT21Uy35nomtqb4Mp8%2B8cfWWMXyfEAk%2FdLbsfSbAE%2BvvNCDCxQRk2UA99wfUnF%2BF8uTsbsf4m%2Fd6iTxzr&X-Amz-Signature=f04a349c40dbfac1e4d57a5611f7babf816f0dfd9477b8dd6e82eebb49f3ced1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

