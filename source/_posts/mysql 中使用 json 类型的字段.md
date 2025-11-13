---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UQAZ5URZ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJGMEQCIB8HIQc7nB0sKcOvyx7hzeBWpxF49LUQJ69Jd3lSsyKAAiA5ePVgTOaWYNRuGxMXC0xU1Izv3qPv1z%2BIAc6T8FxQWyr%2FAwhFEAAaDDYzNzQyMzE4MzgwNSIMmzOr2OboNGNI%2FLj1KtwDoqT6qKMga4qa3cn47s2OIauYGenmrOohMufHKpPhljCFqefB2wTHlF9pYTeVainsTpYDjC6igtxBMPoEgMrDYtcnyY5thDeZKL8akEO1MxMi9rSXXcxGh3mcIwNHOTtctINYgpx7uD%2BPGSfxVTat6MXROI29BH8WPUgVfOy8L72TRg%2BCgbUwbtF0SkvpwyWZSdLvPdx2Rq5qvBqdAsYzoNMRE1FICWgYigUk6P4JJrmoUiCc3l3ABR42S4Bm3Q0fs7Ud2AOWfCEzGvF0jMcvFuConeGCXz6wBwRgoh9CBBgi1VcTnE6x4OydqPMIWyyGWNACF4TAdjU0KcLEIw%2FJ9YJ0GDlAPUn%2BB2kndbk9pNuAkD41xGKQUkpfUJuKaS1LcY5a35usMrL9wULDVOLgQEkfkhtbPN8U%2Fo%2BFA7OfzyiD4%2BHK8sgWr%2BDn7wA45%2F4BpY3o66mEulwoJMYlz%2BZDssGyVgkKO1x2IJtR7rSxrZwZfbS6VYrVpsiwzoWFmzy4wQiMUnYw1EULFIvDS8tpw%2B5vrBdvBfB9b3RO7gCIZ48shahroe0y6ztfwczNyIbICRfGAjB3qHAbm4uPDOYEp4WxPnKqwgR7IWVYeaBaWxUK7t8JQuznwSNpklIwvLjVyAY6pgGxOg4dMcLwalVHOEoR1xwcsJtHZdrK1tyHzk6VSNTSXUdpaIhET4jem2WT%2BFCovdi4%2FJ%2BsbC2p6TT%2BQYY03qmBbXgKq6b8W0652hlkhQg5WK0nUiYg9BFPoRSmmWJENGjbRJldX6lshzhYmyJLzeEG1wUQvIiHdzAeoUvHIvlcYZ0OiznxNjSp%2BtwUt7zS61%2BFYfy8vIydtffr88PipnRUG03ZASmN&X-Amz-Signature=2cb5d387bfbb42631f6e35f99862ddb60d363702f034be1ebd940d3301508960&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

