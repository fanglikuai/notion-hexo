---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633O5X765%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDmu3lP7QrLWYopeC8cI2JHUPUQ%2Fx%2Ffk8mOfjHL7KUjbQIgZ00JiPgnSxVqoOZ0MZJ7h6vgBWW35lP23kLwsDh2ZQ4qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAiIyf3o00xve2GrlSrcA3vONDuXvKWNmykNqK%2BeEjw5FffHyqM3357aHSRN2sGTRGCEAnRV%2BrZEu6JJcBTe%2Bf34axLeQ6ZkC84SmGX8YLchakgtxeU29Ii02ic2F6n0TGogiY0k2HFDFnw5Z02X%2F2x6ioXCfMAaPBSlR28SJCQPb9aHb5FG%2ByczByLd05xk7eMzTKghR%2FqFVuTqyGVmReKxZ2peyUcb5A40bL70wXkNL2%2BFPjxekk%2Bk7AFCJcKSZwlwL24wLzKY%2FQyc9nvxGr4KSsfkWmKJatNr%2Bct8m8XtpvpVudYy4LWeL9Pt9Qu8tiJgokEX3BPF3ylGk%2FQQTupomsdRvtjsozJTWSchzwlS9b2%2BWAGhXwUZK%2FQ6gzu9%2F4vvYNaYF6a8Iiu1yCXxIpmJrCuVlQDIhBpyTA%2FdB6ilaT1o5dAt0OfHczIH6QQvnDffDtyWSSK9BWxGYYY%2BJO1Z9vXw0wUq036g5%2FJS2K6HAsaMmEtRKhh7GFQk97YB7z4HgEtXML3vTffglI6vBt5fhvxuZAxSE6cjuvK%2Fu5m3nlZhDmDOQMrHXGHYHmR%2BywdPV%2BArAJIHhR4%2BXWWVuQB8wIEVXVf0z5C%2B2LM5kHRK56mJXgHxbP5jpDGyPynzUE3lihmrAiKj2k4pMPOEyccGOqUBu0toGS2rkH8BkpDOYvttJIpx%2FrtPGN%2FYoPy15WvAtnMF19AB%2BjecspUbtjQqgkn6e54xZ3%2BkRPY4KtXy5PXJfjEjgThwbKH8zcz8m7dXS4oC%2FFbsbCu2fJCgn6zzwsLVmt37nRT125y9dUFBm4leAuHp1EB%2FJftf3sT41tA8mffBqjETN7c%2FIYfmZfB7%2FYZig0iiV%2FvcbSD%2BEdcUeQCpW2c907z1&X-Amz-Signature=a5c4a6e2194bb06ac8b3dbd44f7ff08bd99f9ce07c66a66214e33c955001ee0a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

