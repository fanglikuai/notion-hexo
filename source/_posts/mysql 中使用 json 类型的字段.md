---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WG3XXE2S%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBGgLHWzYAICqDFX%2FewYprQg%2FjamiyVPiF82YlLlHGP2AiBEeS0YPfjFb0wwEo1Igm%2BAOTEjxcv0QlQMKXc6EgYxpSqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0h%2F3pSw8JwiBzdvLKtwDkFgXXm%2Bf%2FchblOn0TD%2FckxGdFkfz9ieNhu13LSs5JRI5iHsAzQNnUPfrs683ZzoDXe9q0eePwq6syTO8Ur%2BiZbRjTAvW%2FCM8CSD6uGk4ux06cBhow%2BzlRSaH6atiJgRG%2F59ZhlyBg3D%2B5uEqGIxxuxV63d6ZFJsaZ9CWaGmVnbs4NIJf%2B4PVH0pGi3uhTIV%2FSH2ZXqdlJtuNkmyK5tHLLw7jlHlsJFHAXpCgv1SNrBlXiPmRcVLhNekx6aSILeePYEGLlpMKk4ivPrR19OOndi%2FOLKBjdT1UqxBrhomqidfoiwYZm4ELTBnYZthv4FeqUVH3BfSUa2hOymEHRNSWTIQRyrgvjRJfR4qU8YJBCfqWzSsGDD5J3p88AOxz42PXSULq5tEVQPp4BS4CpiRGeavs0yg6WPQq9jt%2Fdh7jwVgQNohDJcpvYKk4DWA%2BTUBFxuihb7PuPUyCpixqSJMURAw2t2ppkoSZS7G0Uxlv2BFipB5UNcUWR9u5FxZrY9KLZ95tto0iUGCq53XEZUjdGUO9IxxZt9Jf1fSbflBcFFNesyHHkBzQlh1GU73Si0sDQStMYXGCdl5VL42OGvj7mFE21DdcdBKX5KO%2FbPjgKwsG9CrnMT9MwJzLokcw%2BsTxyAY6pgFyibE3vxdRjaUa6P%2B1%2FDajMnRBKAQbyuGKd8EVdhEAkpOHVeqh5bB3YCPPeH1fsttGpfhbflAKRgADqWGkyWS%2FLyQ1wx%2FeqXLeTVK%2FwsXlVCeUaOwqJMBxUEwTCamOt2oNBZg8b7PrzQWaX%2FY13%2BJ7rjGW6JFZqk4BT%2B7v8mV0PLjBokTR1MI1jpI4eZCUUc3suHijiHiv6uABwqc1LEoT6v4zjBTf&X-Amz-Signature=ff970a4ec43c5185ff6c315f89baf82fe5ca4557472913d256b434a8719932ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

