---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXGOUC4Y%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeCU0n3Y6533HsA7T4JOwZjKbi%2F1xFbOr%2FUakpeRs6YAiB%2BbYXY3ydOlmxTy9vXf1HUuS7UMZuEJt0w%2F%2B8hfgRfsiqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMAyWJ8PynVPY2z2CEKtwDJVjlDnlGgbgDwxGPOmb56kvKGM7YHK%2BoDQ5CbTAeL0xqxrZj7QM%2FQ00pO8T%2FzzBLQGcdwyL4Uj2eTxH6Ilz8VN%2FudKl%2FXdXw1BD6FK6F%2Fu0%2BLjEsRXWqz5eaP0fyxxnBihgJqCPtIc6C4OlTh4DvcrPEE5y8mVqG8enTau6D7KgJAeKIVgygfUB35%2BkItcY2NW%2Btwia8hFlW1jAifmDXwK9kJjkLo%2B940VajujHx8tdGoViQlbQYniLmviutqx3QGfofN6bC%2BzK4Qvc1NNilfhXu1j4f8TBMuDoMexvPv7URe0lpSRazS2U8y%2BfObAbyyGv0ghn3lSGFk%2FlXkWReLdr7jUEaBbexAlC5Ho9pQQ7CNkNWcqUVOgZXbip5WDJq70uOUHXVOZPhtR94mOOykO2JwkcaVNPpFB%2FioFtJeHhHchDalKnvpNczwXBml4x9IRbf99TEAQ5Pys5Ojr7WedcdrKPMSBCWPvdlrvfvvbg8MxaPdsonoQjm0U54k71A9A%2Fqdk9CeUk1mLJ5CoToVbHYAcxzveRQFCHx4dlgt12Ertkyr1nZLTskZYpTRFIqOB8hFpG9OYPK%2BydvDePquObOlXbD9ba0DPEVa0sMTGd%2BxPDOaXPvFlFUPIswxNT8xwY6pgHcTIl2OMb4k1qeqF2XOorX4d2qJEqP16XwOP4GPlWxH7eQFiVIO95BgNLQly8ix0crddFxAXF1OTH5VB4YkysNFVuc8oparSFdRspB2OAZzXV8qz81tAMVrEGlayJ7O4%2Bn7LKqS4RRBQHM3TJzy9NRuR335ZfryTqeWS9%2FosVMvImQkqFD7pZFyzcRO2q721zhL7%2BFGIEvp8o%2Bk2KJFr7PRxUzMAmO&X-Amz-Signature=9b38de01fa171581d2cd36206195f64163819fb1ea4fd797d19463506a239bb7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

