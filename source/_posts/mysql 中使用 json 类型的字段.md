---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2EGBUXN%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T160046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIBEfHgWMuS1mR2hS9k03D%2Bep5CD4dsDaqbcK67I4%2BeSSAiEAqOQAgnOk38%2BKY8rqmKrdmOcWbkjeH7T56LpRWqpYdHAq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDE974NuP1mUwqslmFircAxOaXEcPqctS4HESQ8FFNFXYEiecn4QuMHHaBih9ires1Lra7ZK6CyXrWy4t%2FW7BFJ%2BRBzt50Q4RKAzk%2FRl4WfqBLB%2BKnOHRRU9f3wvvDxK%2BDR4m4ol1L0h5MZDaTG32lwhJVDkrQUojPx%2B9Dqb0cZM39DqL7lFQ%2Breg0ownLJ13%2B37WDaVJI0%2BQ5d2XQ56Sd%2B%2FbAbR43%2F4i6knb01SSylwFOPHqxL6VPbMMmk9s%2Ft5IGMnhl6LbQ4p1GGmTyeOnSjjcEKHo2%2BNhW5nyPppC0VqGHFvKlCvZJza18ICVzWlISI4tV1yc1mYbt1YHjsGPxhWkxk3H4RpgPDZoxnusum8jUvHy11Qrq8W8GtJ9T0XAwTsQ0bng1p%2Be2%2B%2BiYUAEKSN%2B%2FEnl%2BIUcR38aZP%2FMJZBEP4f3xrA0iGjXQZQ8AeOwrFjMcqZjO%2BTcy8jyAN0xsU%2BYoDloUnBiayOj9Vk3Ge3i%2BJXHE0MyboG5rfCeVcHMOQTmyQVXU0jYYUdLEzFphqnh7Sq0f3OTvpHSsyFNkgtqm52IarpzRvIwOgtQdS0mL0ykSrEAufEBrsxOlfGZoH%2FT9YmZ%2FWz7eQDbhUV8mb%2Frro1IKR3W8%2BHSksUDQJNZ%2FNTAtIP27igIJ4GyMJjhncgGOqUBbvepcVcJmHlC6Z0IzL70tyZdJiMNNSpSirlf6fXKagxcZM%2BG5jXYzOeakVphlGjdNlFs8x8VULDC24YyLSeqzScL%2FCF%2B90oDASh6ka%2Fe%2FXUHt2tQQOlPkzDyLMExo1YvT%2B%2FY6AgW9M5mfP9VFvqFwHOzUDMvFtpOF2IJTSwoqufdT8KNnp3jWRqTGLeNXKNS2mdI7ZD5indUQLV1yNoSkWqnEKI4&X-Amz-Signature=f44bdd3acd3725e82f17f3aafc0ca1f421bde1d87c8a42ca3e6c45ae852859da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

