---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7FARLRT%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIBeDmklNDxZ8%2FFOUTo%2Bcj5%2F9cVnb460A4OqQxp64GkiDAiBVJKgDnyW%2BuWRp673DxXJSCyfNRU3t0ul%2FYREba3GODiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BsoWh%2FsHzSUuXvnlKtwD8UqnWO7rDqJHwhBXoRkpByDxiwa%2BiyxOelSqyEws%2F7vN%2BZC6rIZA9Za7hIFK5oQmDn0Pua%2FGkzKZN3wRl6y9LPoZ%2Foqk9Clh2TvjkDlTQChX%2FmD3FauyFzHz%2F3OcaITcX9gm%2BMruVi1Rc7WLg1REmcGrqqQGuojr9CWm%2BJJlJTrvHkOXzZvkd9HzslcJItYLnqtV3iLdb%2BwMoHwYQauIdB9x4QlyIBOmyqfO9QumKnS6mAqHg2Voeo6JGwmlGFm4KVOHCcts778wTWh6RlXAMVDdkI4HvOH%2BEzdC7QBvY3Q7i1sT9dbOySPkBIWYFAPWXKs%2BiXLGlko32o65XIL6uNDUbq7KlRtDeFUFQXPxy96OXE87ujVCOxZs8tNrJkCUkkO6mB8Hr6A8MyGbk0M%2FxQrkSkssG2ul4Mx4s4DTKmrkqqLqjE4pbiVt0%2Fj25R4MzPvcSSMg6KgOAiyoisoow7Qauo0xQ5vT900Zxv5vwof%2BB1Q41w21xQQuQMHsL%2F8kit%2FWFva7wQ9jNJlmfP35v9zOKlNX5%2F9mFit5yZS9CaOEV1cm1NvqajHOXF%2BlrF3MAHhy0%2BXFvzDdiL%2F5Y7sFOJe1Yf0O9LOmHcd%2Fg5TPsG5fLIODincD3TEIBKswu8u6xgY6pgHLcVlkAOyKy5YIY0Xwt4f6obIUvwG3I8JjDuuodb%2FNvR8LM7AJUK2A2sdPtPau37kLryBGocp1i2vb8kjg5mwmY9h6fBk38IFfpdbHIiJtAap4eG155iqCF%2FF%2FK00fIeT2kVMlu7aPb%2FxOcrFQ%2BQhZwm1jL8QAGn%2FY%2BKTtmKYFCtirhET5JjNp8Q9VMIxps5vc8EMFtHGYPoSn6H8hTy9hivWFmZfq&X-Amz-Signature=96af9506ae9ca164a34a4a4eddeb70d23e15e7562cfca81e1d586365649e521a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

