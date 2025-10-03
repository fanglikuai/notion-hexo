---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CYGS5MI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCCRUFA76Zo7LAt%2FoOTHrj4UW0pkx6W85ZKb9xx7gKSLwIhAOOVIJDRKHeJdS6VqTu02P2lnXT4kkV1C5fcIrgGseyxKv8DCEcQABoMNjM3NDIzMTgzODA1IgxrGhlgiyoPSbmAraAq3AMgRtew3SCttCKgF%2FEPaOBYCxyhXl5axWqKhV4wt252%2BlckXqC3o%2BCL7D0YfpF%2BY2E7uS1o5e2FMVZf%2BhSMiiPBOWzj89TdG4BLF%2FeKj4mAsoHiaXjftBYYECk0uwEBkwVzxKOHFPMb9F0l3Qa6sHxBFQyPEeY7pc%2BMOhp1M3P2nlqdKrwmKjwghiYkYZefq14TpvmvsKUAhVaqGwprEoo7TVMbwHLtn5kDaqqasHDLmK91JGaCyqM1N7s7FdhKCn0jVho0Z51pdfInoFjBt8Za0YQvU9tMnrx%2Bmj9VYlTYwsWiYMHRtt379CyCNdHGKU%2BunyFBlEsmd7WmjfKUWRNAUz72rXtpv0LzaK1qGfQgBJ5IF1aodKC6tAB2u02ShdN7linxV3rjSCI9nBwHB9flpERviZMKd6Bb1vreNMbj6t9b37YwRSF8yQ3xX22TvRKohkbClP28gLkVlUCC1XaSLXupb1Owo%2BCZiqgWjYnDSrkh%2BaKO%2BTxFVjlD92%2F8%2F42wiGh3UuSffoFubidyb6Z3MtBkLiKUMjsUEbDtovh8IT%2BDx6lRWnnk%2BLJSnxqu3%2F2fMwmteC7AyG3plsa7EoRyKuGNaR%2F5vAhWmjR90cYvl7w5SLV7Bwcbg%2BlcXzC%2Bs%2F%2FGBjqkAfYFyQhoK14P%2FE2i%2FcHMXGK%2FyBxjEm0exoEbhLmOtnzghCrW3%2BioZB4I5z%2FP4IfstAVPJpJ6H5EnDHgRJWK2rKdQEKlSPwvYl2tI6FXJY%2BewEI4%2BM2Z9OUV26YP%2FxiADXClEQcQq5ssV8dDv%2FY5AgxR3tD6CooAs2zc4sM%2BWljGPArwckxnJPNg2ZNnMNNLJ%2BXS%2FmFoSqk3wCkyQKe%2FJsevkL7A3&X-Amz-Signature=1dbb8151df4913dc0a07146fdfd3d905d6c562a874673af44691aaa7ec5202cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

