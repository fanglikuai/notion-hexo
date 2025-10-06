---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663L6PIL4I%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCC1haMb8PSYZy7zVmRu3PdZlNmuofpwAYk2vI7C1zLNgIhALu6jOu80jMCIFhHDLBETJsaur4nePtIMefJFJFOZATeKogECIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzDyCHanGKcPeQ2g60q3AOgatAj%2FEQSOVDxSdEwE3heet7%2FBQ82P%2FW1PzCzBrjDERTzWtY87CKci0Lx7LHQPKurLf7Y6lIITVlS%2FUpNUhjGbAwYhIe%2FUvr83oO7N3d8is%2FxPA2e%2FWxuIJY7GXYo56YXLs8Gj%2BqXNbYD6x0EKWiu%2BKyilRtu%2F5yx7aIvl%2FuGf04k8WTHJsn4lmXqPGg%2FWj5%2F%2FtTnJjjbK1E1Wc5McFjfuuzqiG09XJ6DNKe1B08cMzIiiKt1ChaduKCwWdQfT8ElnNVWJJWVbzGGym7Q2uFkm4rM%2BDL13weuRqE5DT6qacry23rGlBdKd2S0Z9QxesJ5qjm7qxgEqTwhA8xNWdORYb0nZpJ1FvH5ilFEn7HAdrv4agRq19AfAFFbLlmleay76Juuk8FxK%2BHX1vV2b4dQsPT9l4hOWFbpBt1%2FYOsjZIzgcvQBby36ae0oZw%2BN2f9L7l91GCWAJaDJLpOqBcgUDvgeFu2KQHds%2B51%2Fy7WPG1xhr4ojNrZWrjckaUEGxyX6ANYAg%2FRsoWPUcrnP1gwfYf4YA%2F95FqYZvWj0EYJ46xA6xlwvJFifDJdYJ2PHppRcb37bQKDxMHz%2BRCRVXEGFTFAHJnhNJg0PG6PFbLXwlUH1VOtxvcbfZwu6ajDgsI7HBjqkAaIksmO27wEPUQa8fgXL1gyM91hKJ8uKRddobYZsGZYhmRJktdLw7GMMch6fsgMaitHDJgiElQaRJ6P%2B6cuxzRXgGE%2Fyb15hTictBfJWyaqiBX%2BARUDiDSkSxcofIy%2BL3wp0iAsfCoNhRjeKuRsChDpEzWhd7VnZ8%2BRx%2BQRzOoGm2MYt2RZ0c98bzYbs2AJaawTirsj2a4lu95uGAtAGhcAcUkY3&X-Amz-Signature=d9b114a4aa5024182a35c71f6b9ee798d0a20174714bf2fa29a0859d9ce75c9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

