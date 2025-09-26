---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A7VJP5I%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCN3QuZ4SEx5DkjGWMW5Nmc%2FW3yEqQRJQCwS%2FmSCRngGgIhAJ%2B93WnGxD%2BoIDxI0OdAnwiqlhyYM6YS31ilDtBqpIwKKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx6UzHNovvTzsWNB%2FEq3AOt6Ro9tdYvceU02oO1wBNK59kF7JjQN49obBA83ZI4WgrIiKQoVBUMm70Vi86uULpYhe5O%2FG98M8mZFH1xptDL%2F2SmYvxDxBzMixQ2Irl3036CIwzVMK3LicW5H9iTj4GvNi3EM6nTylikPwpEefjm1qMxtbY%2FwPx5W8NusUuObwioXckgWhoNteWhtFA5P4PvSrhYa%2F3yYaIVQDq5J3i%2BFDFs5z3YSG7j%2By5nIg5CsJsAccFytCXNGT2fTkyR8BRau0q7aryOTeocEs7Cebf0PJ2OJ6ttdaeB9I1YyaPun3RmaWEaLGLajDlnyGxyA14c9qKaFaRCxWeCiq55chRxRgysKl7Y2yS%2F5KdMHeZTuv7xqcFWzUg20rT%2F2FJPQ04P7n8xR9ylvL4Y36mri1H7ZINoHGCzpiBps%2BVf8ijWpA0%2FlC%2Bmp9OkN11uZ7P8lnREgYPP22E7r8HgBKAsDUQYW4ZPF82o16yo8qwmBj3Hz5GbPzQ0ZWDToW5EtZdX6plRZFW%2FxcAbGFjoHbUb%2BIDNGcPgcw%2B2xKRgtMKXxz0CIEb2mPS%2F8LlEtuJiNYP1Ecr2L2%2BdRwqrwzCX4RFeMtOXBAyh4cc7bO9yoFDZOL42HqjpHSrRkcfgnKWfvTC5u9jGBjqkATXrId9M4SHAX53%2F0VRTAnWrDfpzlyyw5x2GKW9pfbh4MK%2BEoogkjdajKSqphsiWDBzFUiJS5Kcc7mkF00P%2FvTcx8hTW4D2SkfWJYpDOwP76gNSJbGwDPXjM3DNtpqgT03OQh0QRb%2FyKrn%2BNxIlY2NjgMaw2C4LoEHe8XXP0blKrkzbaP4v%2BFfOnKVPGFmp3E0cDmmb35KcAljK26oPx4q6sW3vy&X-Amz-Signature=93fc37b72e3b91211048aa43856eb1c33b409786ec4b5beb7e114e59f2a6f210&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

