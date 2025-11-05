---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XEJ2QJTL%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3L7%2B71515DZxueFKLhNE8F0ROeGXXwZh%2FC2MG0NveNwIgKTXwF8Kj1wBP8FQyei1zZSiUDQpRYzcgjaYxlmGumYkqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL7jnbKe9m4cCWsNEyrcA%2B3W4OVrbr%2FzirVjweOZpMNbDz2madc8aZEKX5LsANC6wpLd1o9uMHZAdPa4nHsWrB5%2Bz3tNLxAXntwuX%2FBJ46KPc7dgXsLbMS%2Bb2yrYHfhPWCtEisH%2FMjnda7okaDY%2B4A77BKaRTpQWq04M7R2mSgB4PTUSzAiO5cvgS%2Fm1uSSrxhAt3%2FZzXbJqXhbaqJZSq%2Bp6ysGeGcTaK0ti0PXH9FXeaX8fSFzR1GqeLONqNJqP2luSMlPmiUlstb83Ww3u2sHEyGaQePEPvddSn4jbXgs9HmuU8iYybJpTWf6T0ppKnPfMSIyjOs%2BeO1sK3mXSNbJpH80V876gZ%2Bxm%2Bbu8KgEISUfimgZRdGNTwCD8zxECk4FJHIew6lYBq9SXZ15NXc3%2FBSxIxHfzF7N8ONkcbZzHFb%2BKXk0KqUpYmtrl7W33V0y7DlxeO34IpJaIYCqgv0GfNcGYiZOpo8zcXuseouftCoJzeBUhPEFDixROfrdE7FaWn9hF4WoT1q4gYSxoMutyNS0KGY5OtCas7l%2FjuJb3d0%2FpROwdKk7qxcRwuG6mLvuiwWEHINzlhSW3cmO88akcK71VINlfODIidnI0RtjYV3ISQyFIu0Ur8xFSozfJFD0nKIGALgL%2FHf2fMNSFrMgGOqUB9yTTAEQEwBpob9pMmWuuuHRTOPpneXgdbU120xrU%2FYeLTebcQ8V4GdZ%2Fj%2F2wLSADP5gSGeuZUQkHRUb73j4fuLqVBpEnL7wovXxoxuvqsPwcakMamDeTHEGNoEVANX7f%2FS%2FXHi1ajM2XjlZVDmV2IH7bJKNHHsrZphhVGPStpDfnt%2FY4qh%2BomXI3ziuxq4%2FxSsw52ZTAHY%2BfTLGjn6tonGJmQY6u&X-Amz-Signature=6dabd1428bf12c963acc97c69107b7a6db99d54c22250bc62665341259861ab3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

