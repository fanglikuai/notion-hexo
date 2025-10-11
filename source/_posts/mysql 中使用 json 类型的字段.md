---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZH2XGHA%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T090140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCICu7se%2FqKEZGxrZDNnN2RFSEbVkNdlKgGxHh%2BvNcaybRAiBEorfZqLKYdLUdpzRAp7Ac%2BqXNacA9NjhQU2ZEjmngniqIBAj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMljaLBpB%2Fq88kseVSKtwDNct9FZxQZWS%2FAC%2BoWeLQTXGy9cJSmxTQE6AOm3rv4m2jc2l22EW0rDCmoRLx6%2B8xDsSqNOtMyVzaJaWrQCVAuXzr1aPvqQd1GV5wmVPHDyknNCoBhfjye%2B%2Fwf3%2FOzopw5cFmk5pdw0wnIZYo%2Fb5ZEMVGGTa4DvlejNs4RSlp9Fv5J2bxkSK2%2FCZdj2HvIjJOQFnp1zEQl2rIpk25quUbHpCjx9VYqwAJicBc1sthDMopT%2FUt%2FkCU5nsKivwIfmdszM8np6el6OmBKuZqNlPfs3P06xnHZlqSNe5QDvDf4hBdRIBZDlk3XQZqLUkBlI4ofMHkGfhszFUUY8ZgD2oLzeGUjmH%2FAsLbHKZFu6tR9H5xxO8U03G%2B6WWGC%2BoNeGNjoZFWURvwBPx82JYDMlaNPZuPGiouFKrc1BNuTYbX%2Bygy5MeFZ7BCDcnDr%2B5lUSAIWt1uk2goaEsQjrce%2F2jGi4JCFnPoxhmHDK7nRhtIJ5640kOvAtt6Q32Ip8%2FgZIk8F%2FkXv%2FTGKbYr5CZpXyoemsdidtaWvI3fykVI8mAlnT2n0orbAknQpmG28kUjeemobBtIlEPC9FFqjqQesFp%2FEwlwH9AWfKHX0Kk76pKBtqxaHWFyBCcVqfl%2B%2Bakwk%2BKnxwY6pgHOUe1g5Osm3%2FfQvWNRCnYyksUDBeZvBCzFW1V2BoVhzBUW8K%2BgX89ZDecV3RJRR6i6zhdA%2F4QLFuYCXAHTGnCoqGBUUHX8tsGsNn9TRZhwb2%2Bj6WXlkN7wtz0nxNq%2BgvyJmAeGww0qpC6HrrGonXSwxWw71qTyfW7uutcplzB7b5Dh4UVlXSP1yalWRpijZF667W8ZebAjADwUhPrJhzvVMbVYHQqE&X-Amz-Signature=a5e5898d876db52b8d277ca73eedafcf4f3805021fb4a73fb4ec639079a90481&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

