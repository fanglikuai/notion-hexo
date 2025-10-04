---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664R3OLGC5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDunFpt9FNrovL3hhHCV7yzCs3oxDa6RJ7FEqOdlo6b5AiA%2BuPMPiZfkrbySIoNWUW8ySMfMFY%2BCKQ9tFNi0Eu90Wir%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMaxakw%2Bk9M2KtYPsgKtwDDRjOqqFJU3zOJIO7f17F2Kl4Q6DfOByCsikl4dQQp9L5Xb%2B0zmjOaOD7co0X2k0wNMRx6EbIYFI1Pct%2BZ5F8tFqbVCHrIf0UBEb%2BNyVCOoWgakrOujGkJJSsGlYsuV%2BYfKb3RAiJ8Z%2BwjpeKrihyCeQKEBRlP9j2%2BkrUNdxT1yKr2w3oVXLekOVx74gpc4UvgTbWjn5xomWGLjTUtT%2F9VgXB30IJWgzc3rce1aTpkoxbE0dPsMNDda0GE4joAF3kF7CLAXL1%2FxD4JWBnsd%2B8Ij0n2quMT7DwVGoObiqkU9DfZ46%2BWYhpakrf7HWsRpSw8sWsS0uEVihck%2BYU3JTVY1QGjNmxBPgCFNJVoq1d2DsmnmS4XprURuTxr2eOHgK2fVyQHCJHyzDPRwmDpL34LMcZ6QiHMY4QImOwuR%2B3v5X2UeEmMd8ucToxKP42cvK9fCHb4SjuffqYXjW0YNcZLjOUGG%2BHW4PLNvU5KmU12xl8oLJZoU8za2Ocj9feJb%2BDYRvczKAn1SyVHZ48fjfVfOPXz15BUjKzRO5o32Ikjudd6agXS2l5oXmbnrZmF%2BWiOrhruxPqjY%2BhdG6gMxK6KSHp%2B2grmh5uxaZG%2BC%2BQhLqD2aPDh2Qbky4ZrpkwteGDxwY6pgGGj63paDhFQkm5sD9ej01gTvA4JQzDqhUoq6m64RU9vgb8tMfjS%2F%2BF53ke69yOt%2FetlgOawQOXe6jEyPWlg266KtpmhT%2FIdF7YE16X7kYcOww0trP9GDd8nupuFNwxBczKQtQVJ4D2oW0MsBHwasdsn0ucTSSnOnY5HpdW%2FJGOrs%2BnGWzaUL9Fg4iOXQZsRZh1AzLNkv6GYJ6AIDWJyTqbe%2F6uMU9M&X-Amz-Signature=24eef6a7a341cebc704dff8a2edeb9c3979371061aab41fe1f66e36cf5fb8b97&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

