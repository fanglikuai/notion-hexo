---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46655M3RINL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQCxTL8TJxyXf6O9eZzdSiORMr7t97B0u6hm91z2LnQj6gIgJftBNQzLN%2BaKehcb3Y93dg7wzaKC4vSMJZ84l4KQqe4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOLxfETd0g9i6hlf2SrcA2DLUx4f2%2FhyNhPUSibX3fJZAj%2FWiYcOEMCvckuRDidURLLyUutHGwgclSos7HlwrbxNNIAuJtDKpgutoYEWbBreVzt26ixcHDC88k%2Bmk5XK3iSy13%2Bi9yPQPjgmqt77C0tSBYOVi%2FO8AjJep9%2BsCQo%2FMqz47sWWRTh4HZsO6HpPrAFL2hSDZL9XLO4Q%2FufzxoDOdTOj%2Fo7Dhssz5LsX2lQaaUzEVakYCqj8WvCl0A8R5pz788simeVp6gpDAc7X4qriv%2FF8YvBXRaut1XVZwqkzYjlVN3Tc1QMuAPUTKA1atsMYBf1ZNcd%2BaxVRgJTs4iWhK58yT37X%2F5hayVyKTpRi7tt6mq%2BTxbtRC8wil4i6S%2B4lAkJD%2FFwvdgb60Wkk%2FbkF7jgIrfGb1Xzgv%2FC1PpwLAG2Kax0LZOIx2tLNuOiKsbYdqYVkKO2fSo3n5D7bmmQ7zYJd%2BXLggUYPhkmIpzC6kfbyQJy1OgDI9pGQ3ehkO8A63ewRMajbmZco2eWEr9PW3gh%2BpOso%2BFW6XQC5h%2FeT4YIgNSmxKklQsrxUojd%2FbPbrUHENFLECSZEHcZCYRrvKe50jBgRk6lvO6QhLiSH6z5gddbmvpMKLcd62WAl04cFqVndDwIZJM3BuMP6zyscGOqUB6tYgrZXfc0dNJrwMuGZHRodRU%2Ff6xIhgjP4YDTj0yCIWaAw8iSjj2E6UUfeL6ml6N6wAvfSA3LhdcARbehGmIchh7T2knwkHgHtfUANLzhE1eHR9gW0dOD50RsWAq1%2FfMwA16yggOzkRUM25vTJKPm0W7gntuQ4l4LN3H%2BGX5VfQwZ6xEKOLXM84HtlV4wjq0CbbleiEMkuSNqPOylyEYEmCibJ2&X-Amz-Signature=3b4ce7a52702259e5c7b5889369cb82db01dec21b42141740f6478b83af6a0a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

