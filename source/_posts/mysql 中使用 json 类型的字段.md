---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MUZQGHQ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBjgCp84ZQ4AkqnHFAZdw2QBCdbkazOPuLtg2uIOzMQwIhAMk3Qb8aFoh3jVsQ4NPlluNB7HTjJqynxhvB2m8FM16LKv8DCCwQABoMNjM3NDIzMTgzODA1Igxnyi8TpGvqwznrWdUq3AMYwD6O673v1Z9112R59JXzMtAsp%2BahtoM6GQvQBgLynEgRnOkMgFtGRLFv2pWCFsTkHuX4hh1Z9ScS8yD8YFE1ve%2F1Fe3esMBMMmvbydwjwT7%2BuRVyi%2BDc3s3Ou1Iw3KLZzeX4m8QV2toEBhyFNQm8K1aUeBLprJeUnUIZYZjBW1U3I8MW6icn%2FRKYTFHfB1JXaSobCn%2BeJuguRj8aUcVsJIl1xD%2F3r4nKWDL41o5KEBnQBxhjbT9dSLYZgiDwoON8JgUOz6KcheDMHNwcBGu0tDZzEQr%2Fg31moRW%2FR1pv5MucMPdHVaEZmg2KS9c7e0jBCL8OJ5SaFgahs7fMJ4IiOv0H9VTTS1AnxilXBan%2BMLLZ%2FzEC%2BSTh5Pa1ex57Ck9RD1twKXrYcB%2FF4tI31%2FF7Uky6U6k4S1QTDvXCKyNHy4QdgXRX3K6517TSjUfSo741kFMEk%2BC1TkihDsma7hyZ2py7ThyMqVFwu5Ego%2FbaLddiYoxntrYh351Y134%2FCfnDiGCZ51sWda3NhZo%2BKWoQu5ek6bsrMZceHL8O%2F7fYe6Lct76CoCTUu%2F%2BAbxkfTNuk2fByJfFyqTTpA4qvXa7dAs800Rag3ZMlk9IAvmm%2F5L3LNItvHUXqO79v4zCu0sTGBjqkAa5Xx%2BAgsFj0NI7CR8nED9FYtKg%2Fv92MYDP9H%2BB2Xb61sMTFz94t0f9wVbm3nzzhmx%2Fibp9cksyOQL3EiaUWy0U51Aenmzl48JsvQS5nMFcF0WbPPmCcC%2F3kzi7zUUzElmc%2F5YkTbzicV54HNxs0RAdw6WxLuwd36qNKYav1QEdQP2%2FYhk07SA%2Fqbimd82XKXQ9oofBE6emPXnyO8cfjn3k6tj20&X-Amz-Signature=55e33e4e931026d05c432aa358a93d8dd6de77e4bc4777a8beab5d9ef033d4db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

