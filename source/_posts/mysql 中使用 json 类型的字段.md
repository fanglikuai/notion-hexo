---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3SANYLK%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJGMEQCIGegAY%2FJ4ODMC490UlFnzey1Ex3qzBTGn5P44Gb3uCqqAiB17W1wDBgkCqxoTdWkQPNX1jAyCThm2w5w1BAGaxsJCSr%2FAwg0EAAaDDYzNzQyMzE4MzgwNSIMiYuVjbTgv04kBTXJKtwD3GYvV5GsuDbUg6shKA6aEAWOgzJqAMtQXlc5U7bhM0Az530EtCGXQ5ZGSL5Y0MRJW9OTuaV%2BoacKhNDnmcAUnYhCqTBXZgmFrSskKsCfDNAZM9XIJS8VXt6fijaTPr1pMWPrfcNYFQBAg7V%2BTqTk%2F7s%2Bch1QBH0%2BI8ygt9RxFG0aXUUKYMLfFk3BpI%2Fg7FnPm2liS3r0pS1zSE4v7Y6k2IStgvsSlHjx5lOIL5s5i8u8KXBMfr0%2Be%2FhT4h5izmQYwFGB9ObVEOjmSL7H8gdxMSAjJsVFT%2FuzeHgL3JsHALTdGU3xJAzgwnz9P0IGM6QXcNO0wk7zqh%2FxvgSEjq1VCxu4ROJjLxQ68qtjm1TAEufJYfIByuvJPLjbDMA4DsAVo0FzPWyTtJ%2BbrKjE%2FsZP8jEvYlDhaJnnDIQQOECqxCywQOZzZc5vnScoeDlamc80gYwCuRMbwCZH4VlQRs3WTN3JO26Dr5rQLlm7nVjUc5%2B%2BwEq%2BJzFO2jM7ZRvBS5MEtyhC9jfUBwmm8sPc1b2%2FKmSIHCEQOZS90fJ3tP5Wfo246rjeP8XD64XiSqrqManeC7c58zXtMEa2CjoHAqzgrNgcU4gBRMtdHrok%2BdxjrrfClQSLheOnJdMfkTMwudDRyAY6pgGz1gWpKHsq39ysTCC2yE41iSejrNrJt4MHOh0DrCM69WQqMU6wQbHKXlQT3%2FglzFmljvUUBp21vadHv2Ko5rVdlEwK9uTEHl5P15phsRPWDDAJrL53kH1H9TOsix2bWNKUOS1fULoEzjWYyV4zCL%2F8oDAUhDcgs%2F1I6jP66Ot7M5GJeMULrRXimetZ7hOLTqjZbN%2F9oehcxtNLmmV%2BYYfKXig4l05F&X-Amz-Signature=68be44f2c4a6e985b585baf337cdd26c773a57192adc5b7a2142cdab18546597&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

