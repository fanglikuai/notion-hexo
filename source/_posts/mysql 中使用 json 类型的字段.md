---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EKL3THB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T210115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDE%2BhBhlEyt8GEvijMxuaLp0RAUeUXTZlA8BkDkZVgLzAiAVJ3VzfRSUTehd9oSkh7LVoFWrcz8azHpOiL2j%2BHkMdCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMO5WzsoDSkdRfJlGOKtwDxWXNimbqjxNmu8hrJbVi6xoAGLfWhj%2BuxIDwGGNVCxWrt5kg9QuowXhf%2FcXuDRyZUe2O2Xa6tU6kAikGXSSw5fvDv7PEE6%2Brpd7Dnh20KUpfovk5yjcSJGaLJ08Pxsk2kouXz1SudyPq4YQ0Q0Fr7FQ0y55HzjtmpSAYF67DkXZG1xlyNz0ArIGJ%2Fqwlw2%2B311vF1bzZwysjuuA3SPvddCgSyOQQsOGdYPPWGcBmb%2FaGKutu6SfZDivGj2ORalZtrOSvjterLYG%2FUkS9C8VZuxGLsw3QdjXXZe22Ue01WwKj9yR%2F21NTG%2F1Keam1NSOYiDQBAaNYjQ92m20RHB2kzNdX5cT2LMR7qs%2FX9JdtSip6OLMAmerEQd%2B2Elf0HlBteP0YzDAnmicfxpr3Td4Jzow2791niW4itniXb4VeIz3%2Bu5jatGi%2BodbN5uNEB76VReNBLRlUR6HJR6kv8bun9%2BkDQPfgrcBnpOkgEH6ruYe3trSEeUxwWjMPTe0hHCClSj3q12lI5Wyd7tsTOIFKpt2wW%2Bxj49BSrUsEJROxnQIrN22JTRAgp%2FH%2BP6snMHsvcMjsXDdanLkLJXyzqiCvVBmiHIbACP4Cx55Vaway8b6WeqIPsAU4rbuitXQwzOiKxwY6pgHmne1EJkJoS9hexm%2BOTU1%2BTeQNLCg1CrROyD9XbmwhvKpMUVDGt1QjkhwL%2BFaCwAEOGNuNKzwXRXSrjFzZ5ZLOZfP963PmH3n2yFM3Ohv4AR%2BsNCIwAU0KhkHjiOomB6T2Wg4cjJ3jUmTlBffLGZChNOVNj3UneAwJNACu%2FW9gSliaI%2F191iNlaAGZFU9ilQ8k4SvIxi6pj0U%2FDLkKY01zNm1xHR0%2F&X-Amz-Signature=a303ae2744621e8d35383e66042b836211cd34cf2a5931ea0b5089a1cab7335c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

