---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XXVBYA%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFAYoWf%2BF09uUeGicoqnsxRf7dthGHF3pEmcdD2nJuu6AiB6n0FK%2FHN%2F6%2BOiTxxCkB%2Fvgx6w7S%2FnQsam1MsiJlWXTyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMVockYpFEOvER1csFKtwDjxSpf9SaVsCGvaD6j8oW0cBYKYNn%2BNbddjkWR%2F51tFyLKO8H15jiCkSb%2B5UfmOXtTedIr4hOM8ot1Z96wGNRxkMDuZvQxhYQoVZSYtTN%2FtimYU9V1ezSbHJwJ46MSl80MOzCzbK3rFe433mnp22BmxF1FOQhXdZ0rnjGH3UmnZqsVkPmS7RRrCrFN870e9uEwtbZYT7XQ1RQSVCqc68EcNoB82X1UnP%2B7ai2DQhJqDzkzSfk8mNqpqAtL6QsZr4zmhxBUOD%2F1ca2U3nCTcDnh2fZZ37lUr0xHkWQm2oq5XZhRtt%2FKe9yGsDYM%2F7cTYJiqILlp6MMdSxZde%2BC0EVL7lUCFoiWOugzAaO2bNyYZsB8uSIv5VSUkLp58RIBtc9SyyTLhrDI8x6inHRSmFxTw0I4t8WHG0U9vVfHRVDMdEL46NkQve2Bwlo9m73ltsOKvzTJk2clu2zVixjJVv%2FnbiHDHHCGQPuwOkKvLsCy2mb0yMRfFHP2CBx0tieXALohxm3IkbtYMJYIUBUzTuPrkJjB33oOyTtx%2Bp%2Be6dAqREfut%2Ff5VYRa5kpKaIaOgHlVokHQJ6l8n%2BEOMwa18W2Q7xFTxB0gEzjpJTexT2aLV4lzltSZwVjNdE%2B00eowsZ%2B%2FxgY6pgGPWRM3dht2b8w8ZIWhSHg%2F5E0VINGjOjdnjCGqRU6lLM2I1frZcbpwiAkf1GdBRLmnm4c53QypWJJGITZwjQf1rnlfcJ52x%2F1ulOJH4DPPe9n1f3pqqv3aQjL2PQWW9jrUYkgDD0RqfHBoVRYWM4iQVrCY%2FVlTLHbfsUlnFcZditG7M5wTU%2BQJ3O7iudh5KYtGuQdVr0uS03Nw1zbvQ8c73%2B%2BlXrEK&X-Amz-Signature=58041b91b91d42ef194b009f7f93266fdc15db824bce09139e2dffa1a47dd9f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

