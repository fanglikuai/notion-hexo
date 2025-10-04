---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X263LBGX%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTd9LhymLrklMJYCUjGk%2Bv5cbFAibKUcpUhlRsmvJZEAIhALciP%2BwjmAYR5slXSlXIpvW8f4ixiPVf0jO%2FnbU2tXS0Kv8DCFQQABoMNjM3NDIzMTgzODA1IgwgOu35BjkEbCydzwoq3APs0hN2aM5hFufJfuRJQNVR2dPG59a0AShTgy2fBhscSSyQFeUu5uwdQ0pGTTWq8%2FtDwSqMoQy6DIBMbsWPqkAXRkcQXfvvSSBVzNL%2BZzikxvTYZ6RG0LdfO25LjyD4BgzIXCtpTeKfOZuFCBHYbnMURnYwkJjkxZ9tbylyDG6Y13dECWbGq%2FpafaS7Xs%2Fgy%2BxssudRbaweEFMC2gWMN7%2Bt4flTcQThWWNQqv%2BYhIwJjVGK6O3fLaTTMKa8UoUbfIDI3d91H2Y%2FdkOoK0MSS%2FDYvzxIZnFOfGiL2%2FHQNLTmzNKgaPNT1bcQQoQ7Fjvnf3VUPW4t3Lm%2FMEygh9AwtN3IPooKtZM%2BOjSD%2Byy%2B4AgmlQYHhyBuZ5C%2FAwpCBa7%2BDmqAjaJX97W%2Bkz8XQj0ZP%2BUeV%2FcVE9dWuvbh%2B8nObhiN7v0nzY84VfCJFrPwS8lu%2BulkuO2rJHzOasX7M4cs1nyqVa9PNNw0UTqWAOqE4i%2Bi%2BNDyRF%2BLvFbIO2z%2BzaG3bNNKHztVmPL%2F0ZxUqpx8Y91RV4A8HozXgGV17MRPtyq%2B2pzTcdKS21WPzpk3gy5YwAGnOTmMboSSf7DuTS3rySSD73n0kQtF7JHv13LqXrhKJtB87DF%2BbjM01EzhHDC%2FoILHBjqkAcO7p76TbXGfn1qWKjlK0k%2BiXLDUwzSQcOtLLytxlvCIjjrW5vOKJCQaBZhErM22ksYY6rp8ppZCu7iy2X5i8IH3FF6Cc3QjKwjKT7letqiljjIgecQzOfRatJq7jvsUBWVnIilyK5jKRd1Tr1b8y26LjwCJkTEL0Z9E2d9JSTEGrHbpluHDYuuZimaNUgx8PJPJGfJZmkPqo9ncFOo%2BgYTKdZ5F&X-Amz-Signature=2ac57cbf6bcf7b851fd89a7a238aab6524f456b0ae3e0beaa6a6cc7198c01a5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

