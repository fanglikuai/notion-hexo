---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI7CQJSV%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZucOJ16aHsaNrkXBU9ljQ7z3oZBH57eJCmkNgUjDp8AiEA9gjvgCfuleCKuNiLml6i5YhEPVludPpnSC0PjUfQO78q%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDGfTWOauEKIKmXNfYircA41H%2FqnzbCxi%2B%2F7nT2trg5a0QkdNOpNNbx80uSfBCW5%2BBVimtlyLjgfl1z1SUrNDUnxIWjSQfoCzXJsePfMAYjNunw%2BApy6Pf9gYMiJCVUTPx%2F%2FeF6guPU053CNGegpLut0z3lJgsQQYNpI8jX40t09cCRvO7Gpc8XIV4oIitaE1chJ8p0moCCgpy7k3PAh1ByiyBk3iC0SprsjrqrIAoPdHxr3JwK1cOuicdICylj11gGnVxrLQ%2FUSM7XpFqOLfk6tm7losrO8BmaOTf%2Bw6SR1w%2BlMr16K%2BEwIy6VjplH%2BqGS3O0kPw4d3y1WEytLD7NUVVqhpcRYwuByyeiG685skT5LYKwMfHPjVnWHRgEVVbbbmYkagz%2Fb7IxxsrAGcW9HmKLL6FQha8Fs%2F6CACMzG5iMrYvwox%2BkEGcjaZ6aToJBRRoRRiFG%2F4y4bfmr8z9xb6onENBpxFa%2B%2BZYoplp3trA6UYoRAaNIQ6gZLHn5C0YNMser7Ubc3f%2BoZWx2cA91r0Le0ahk49DFdhgnnf9b%2FGCq0ghlXAffNwtnZpalxRhsLaduUSUS0pLJJdG9IJ0s8BGDGlA0dveZA58HuCM84ZPIZsEW7rUvy8h%2FMUeeZI88FAAL9Dk%2B7GNFhXVMKzFqcgGOqUBwyqwVwvkoc%2BealerELrS7Oo2OWJhwBanS%2BKBo5in%2BADrc5HX4ZKVKIVpHRCvUpD0C%2Fvae%2BVb2sbCbWVxHVgLSxqG%2B07VQSx119JsORSnqWgsQS%2BVMITaeaRF9kP%2FXPADNbLBjLWrDHQYu2UPBdQAad6JVzSs0NZ37uYlt%2BieyhuG4aUfaEpc8mm%2Fz5%2F6lj0erPv%2BUOZUXQSOVzmpFjQbmvr6nqmw&X-Amz-Signature=582ca0063e803a3f0c400a6e8e553562cb38eab0556366ea16cfc9d884f01011&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

