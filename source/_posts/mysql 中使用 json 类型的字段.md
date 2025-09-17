---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FDT2UPL%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJHMEUCIQDnAhyIks9GaygHFzoUOdMStwq3xDLC%2BPm6jCtOdRGR5QIgEXpoSrg211cf07oIG0YBOR6wnis9Fe%2BwuFgKlFvNk5gqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEx7pgX3O%2BuLQh9GBircA2%2BjSLq9F%2B%2BK%2F%2FEF%2B9pRQy6NV3ppSrGvE9vTZgCl0gizjqatpUkqmf%2FF21CejqtzVhhciZdAWylXW8ymk6kVV9gLOQJD21OR0mU4Fhg4bAtb6wrvxTXXD1AmN%2BSIL%2BZ%2Fqh1CdsaZXT%2BnIJg%2FnQm63KywXDjCkUzCtrylRpw1SiZtMKJvWT0jBPTrdweEqsfuISJDA3fv85AZbkpfT%2BfK1FkAj0oJg1edUn0tkAwv6Znv%2FXAj8CE09JQXEosqcHsN7I%2FNA9d1BJVKXbkir7wmtcaZSk%2FJ8XfWc66BabUagGKDqNyYuk6abNnV98Ixneik0nRY%2FydrQgZk2L2TtUpBRw1Ns%2FHW%2FQ%2Bv5FO6nS0bj716lMpdV0mjbNBTI7j5Rkit6kPjcFo77vUlBUL%2BMhoNbljEvW6kb6MSUOcMPwowNucqW3H6pPHiZ%2BJlg19vYOy4T5Y16YySkB4xLuIbITLIjIeY7OkJfkDqYXmSJimZVimBfh1MWIblp5o5STvQnv0k%2FmifNfE%2BPubZYSkaPUwOLn7dn0z%2FCxauZZcoY3g0sIvxfDSTRZ4uhmS5smRqpxYelJTOLcXS7xpJzgayHmJBhOMtiYDFUgYau4UJSNZ793yH7oGjs7zJDMRZTEhoMPz1rMYGOqUBFsVyuhM1xlgJaBnbAmapjJebr%2FNOTEN3GgSFe%2FzK149YVh2PGPZPFf1CtQCFXbMqo8oS8a0Cufe%2BvGEw00nitsidC1UQRZTn4QXa0N7qz4d9nADmTeeXAenvUnTGeZAsSJSYKhKwL1J5S9mucf%2Fo7YKLyap0VmjXVSKuEBrPQrz4i%2F%2F1xpJVPx5FyTJ3%2BDS%2BjokpB%2B3qcKUvSi%2B7SokGwegxEv%2Fi&X-Amz-Signature=41beec64c183cd5eea6ae41eea93054caa5d40423e463642c59a913302ae3a1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

