---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6LPP77%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T110056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFhsyikuge3MHGgVyumxwTYaxedkjIZlCPsarIKwJ75pAiBgmDr9D8Hr0F%2FARnAcPkGpRiJq763q5i4G3Gv%2BwJbFiyr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMl49xGuKI5iVoLuY5KtwDWL2D5vGGxaQCCKHv5%2BUB9lxYgECgHAqvsBISO%2FP0FjHI7354z4XjGDfuUmZut%2BNyhR%2F4SX%2BmRUOj2M9wzEkPvhxdu1QCrZeVpDEGy61po1cuRsFSezu3%2B4JB1anZ4xPqjtoPXtAsTAnyrtmPhZcKALJ2lB2fvxhAEMl%2Fny7Z1ysfRZMMchUPdkR1lXCgEvPauHDo8whM98FurJPeKEHgmlJZvz4cUBKlwtL%2B91o5gd19NCX%2FQNLKcDcp%2B526VAYFpIjJ8DG9vhXcDKJ1as9a0Fs2hlVioVrIBpdtTCYuqc8Uv%2FD5r9Lc%2FnceUhjX3NtQlQ0YQNRO%2FlIYTvNb5ehmurioyCFfaw6riPvqJ6pbxkGuh0Xd3G9hsUuawDOxN2EHdlZDEd5joIOhuvYpZNfrLGwF%2Fed0R4uHCjADkX78FNP7xNy0OhNRlA%2FNc7%2FXl3SLSI1b0iMrfHDlC5chxTGYPHMu1wJdpVSFEONB7jqywVuVow2C49PjPKdEdmkaqugiPDgzDuF6jGsjZnuv7z4u%2Fotc22QKm6ji0jlXOc%2BwtmC4HMJkRYR9ezxeMCvrYKuULwIBhVbRFwBUlDzxO8zWkEr%2BbKA42Wg%2FIFp8x5aeckRK9uz4rugf42%2F2wtswqYKIxwY6pgGLhHxakt6kSiNQii4uXsimky0z%2BZj7AYbD2Cudh%2FGkvNW2%2BvzO4qdt0v%2BeH7X1wKLhf7MGORB909uM2cJeGYfO3IXz7OM6i4OJA9orvKNOOa2NR%2FW3qFsEh2xY0u8VIMXwtErS4ADdHC4vMnwHE%2BAlVs8T%2FPCgo2m1b355Dzog8Rb5h5ih0AkjbR%2Bb975KtIcxqvys56nrdsrW7OySUTZHRVWScsLX&X-Amz-Signature=75f8198c3e8aec8e14871b8bd3860cf5331cdee9f966217d7b2b47dac03e425b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

