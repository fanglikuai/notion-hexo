---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UEIOB7CH%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T230045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJIMEYCIQCzWDEY3EyqTZlbL2c1jlfPApLweTsnkYJKy95LdDpAagIhAKyLt%2BqSaGM5CrdwIUQ33C4BiBaFHAb%2BCqMIR0zVVzX%2BKv8DCB8QABoMNjM3NDIzMTgzODA1Igyd5hhzLA4y89Igeysq3AM%2FybRCH4CEIshhNSJFcQnt17dMDAeEgxlwBUvnTG%2Fp4Fi7kIGDFuvdRkPRUA98d0SufOboHosHez7kqlxyB6eki97UrOm5Prc1P%2FlsAuoRc5385zDzsuBmW2W9lHqxYrHqxk7S6bKMWPG7X02Ln%2BW1A%2Flf5cNl4ksoYAGYdfg8vKI1mTZHJ0epRgErWJ%2F1tmGJ6lRNGdPOa0g%2BB%2BGmxjSyWp75JNBEenUlmD2azF2WTFVuWToy96r1FKi%2FWvxJ73CHr4vht2CC9F17Qwg%2BIE05LgLZEuVUppwU%2BX4YeLuP5%2FkQd%2BQp3amY7M94FjDfdpYJMra%2Bntx2RANOrt%2BzY95u0%2BMA4knXbEHGzQkiVEWncDJDRAyx%2FmzRnDxX6XdL713xM8S9qiSzrdUyeefbuLDuhSqlCWWtk%2BH9siwNxV1D91G5GopW2BoBaDDv4onNOQ776KOJu9O8coSN7mQb4Rx6thJZxMoiw4YxtHtoU6r0APYxnycJkRIN4ri2hwtSQ%2FSovXhrXIinQt9kDx0qq4wx%2FRWD3BeA%2FbNaKNeOtexsn5mTtDc4xucwrb1T69nLi9kRLsVxFFq1izE5h%2BMpoL8cFMY4Q6d%2Fy2Pgvmo%2Bbh1flV7qu%2Bx3UbyfF147jTDfpqvHBjqkAZrB7Nzt0WUjkamo5xjy1qY4d18v815yV%2BiYUZGjL6vGD1qvETmt0uCgV91ZbpTBQRV3w1al%2Fz20FHhUxpYBP9rqRCFjQrKTjUD7Hr1MkNOmky7WX%2BkFOD9Xff0DFOB7GPAHsOwSULgkc01YHaf216cR5arnROToV2joNXYGJLPo9UMym50rE4ea5PBVn92ZS%2BrvyyuT8vxeDZSWLpol64zji3N5&X-Amz-Signature=4565c5a0546604b4c734732aa590ec229630f9e4ce174887c4ac9abf1b0cd52c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

