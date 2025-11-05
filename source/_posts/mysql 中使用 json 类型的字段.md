---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJWA2AYD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T150104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAQ8747xHI31sSyeaeF6Is1vOSjJWFnDNU2hDK1NDKiEAiBkhxyZF4FcBPj3LKW866NeaC%2BgtInYBjWMR4AH1957VCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtjYGQQnza2ksIFlUKtwDg7eASpUnrDQz%2B82Bu%2B2JkhxqS%2F4wBH%2FpF3kAjkcmBtAF%2FqjJmDRa6TC0QiWEm%2FnFEQef3mEWF1rQ3Jk8Ae1%2FqhdoEK1ow0JDqzVsaENQsd1coRurBLCjCTUw6oFidnKglrN7Gq%2B%2FU9PMbWE3sYNzIJB1SLZU68HNR7HEtul1R9JyeZ9lWkvVs%2F%2FkmDvgFec7skj4lb6VlzEMkWaNJrUoJC7VGBgVDwejYq1ytdwhxTN54miN2SNwZaYDQSVM8I7aYpHDgskUDiNIj657nkVfEVheROAYttVY6whyzUHV9FOi76zf4ncRsjK2zBEAj0jj4Q2pNpaoqPxzg%2BUUBZXIIXrtjMvo%2FuTwrxi8El8TbEdQ7jRtRpef%2FXwscqtbM1kRbTwQ%2BP72JAdVMwBEjQvy4U790TMmgaY2FQOubCPmOseH0stuqf2K3nB%2F9iHASPkTDNo%2FkeLc00hIjjhdhFHVstFO8DkP1mvzZ74TOZIIpnTDOyFDHywI4qHN%2FNGbHnnRFskzrFxBoWzFuwJD9nrzHIu2yjj7qEgniW2J6KVeVwcnn4IOkrH9vU43ARHJzRwvkU9x%2BjYopntgXwC%2BFhUc%2FwJ7WKwMaojAC2yuz2SkMS9lcl2gT7E1hh7qcm4wsrqtyAY6pgHtfmfGMw4QCxnXyAEircUyziNRsFko84jLtFJG1MDK05fPHZCSe0EqcI0c8RwQppIQ2I4Bu%2BB%2BDfZXpStYqKZl6xxSSK2BE1Ir5Rd9HpnyBLvrn8P3K5Io51l6mWnV1iH11nGGokjL0rQ8CJAvKF2UPc5P8Cj3JExSBCAkzBhQiOznkgXC0otWe0CQ2rsE7DFyE3IEd9omkvA6sdTHwQqMaTjDEj60&X-Amz-Signature=2b7da76b0b77561da47558625a530fbac7a655303c6a5dd9751375ec5449cff7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

