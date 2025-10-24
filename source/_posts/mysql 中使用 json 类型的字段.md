---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGAJZS7Y%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDBVvG9XqKG%2BDEa2WkWc4zmMPLY9XmrpjtWmXz%2FkSMLGAiAjhiImvdDWFaXiTT%2Bt%2FnydH5yGsGegT0ODAE1n6hyMtyr%2FAwhUEAAaDDYzNzQyMzE4MzgwNSIMgjriIbrWegbjGptSKtwDyG0TE1cXImFPFVgRNVwgwz2gvHVqObcGXIccjIv0T97msYdm18m2WTTC3hiIWCeJZHAv%2FsFoaS3KWwng%2BRDSd5DR82%2FfiLZiCZ8D%2B%2BrxDSFUuHlv9QW8OmwHvN%2BuEOSMDpeJJxIxnQf3HZej%2FilUhH%2F76jxy22XL4cX28tI91CNM5NeJJJjaCqlcRLaPkPLq7dmwYDBGOOqigaLsxyu%2BbtbOox9u4vE9Z0SdLEpkorCNGC3M2BxXtDK%2FWvix1cIeIF5vhfMdiJHdWqBosD9uJSC3%2B58G3YraLJxYjbXSj8Laq1HTnfkdCtwpq6JElrnS5864KCkEFWre9o%2FcxfWTNKYMFZz6EXpeYySciFqhaEeDmLP%2FsiNyq74KbJfS5RQq09VRyB8TgyxV9tm7Jd9TpHjlZeyMjE1XsTU2TyrrAhljNVDMmyszeLWL1NQo52E6UkFECsZabA84Bk3we6CQaJHeacNZgXVD2Clx0F3NOhArJ%2B7Ae0635MXyuDdqfhdXSajiaqDZHeJL0Zbdv3DAotyiHeA5dzyGJJfn%2BL5xnCbaZEageiiL7OVVjyOQzdpXESzuL72Fm0KgyxxkGeeIFUgg8I7MlRyPjLIpgI%2BBUSr3PIUF2MJRFf7qEzMwm8vrxwY6pgFvzHPMdKsG606bsgd09efuEuIIXHVZ5XoVaJwADZnN2aykC0bq7EEpKRQNhagdx3fJRP1bSd5VL3IaClSvW55yuAIEc2RxcDiYWZFvb2H5nuAm0fRcDSh%2FJHrqT4oMWNCmV0FEsPzh5pGKYEQmsk0bWz8g45GGFX1zTg2kzlPjN3gUNmqQpPSNhEQd5X0x0LqS2UXlwBsEUghnnlBHlNCYVI2hSELY&X-Amz-Signature=1fb055a8b62517af6a99a52d2698e48b8bd9c5213560e6b836c04c08cc08d8a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

