---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46674JZJJHG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQDz1jFbk2HPxZ3TnmrTdsm08Z4DeNVinIYuyVIuVTXWcwIhAJNzfaaxZoEhsHw3ddA4pZzONnspjDAZedhwQWaO8yniKogECO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxxCLPPzYWiR%2BHnx%2BEq3ANSzxtXNR9deGFPvMIuUy8JIPiQXT3y7NHal%2FpBtewDLYIykRA1F8lQJptUNCf7GprygeWv7R2o185s2vNhwzLuLP6igjEFouLvLub%2Bjdu3jYlIvf9n3Uwa1Qdui74dm4YUmJf%2BoHK7UiEXP8Tpw00YcitA%2F5Xt4v6pBTspgvfUIXoM5miV%2F72EM%2B23p3lZjqfK3NhNqaMlAtnQRb%2FLaeABxLFB6EXxq3LF6EYfjGEbkQKSrumx84erhjnQGkO1vIdryCqCrWYTdzqRvJahZdDNJ8E7MjUlM1E0wni39lv1axWaF9KxkyEjsVAroq2B5RdFTGilg0elgp4ZsewTU6uNgwqYx78wluMWJIb2Cgu%2FdYSitfohourE8bUyXZEtvfAyVA7uRhJW%2BVi8gt%2BgEcoLJ7paVt%2BFtZUy%2FyYt5RzWHwfn%2BVrWMAoMkOY2NBK%2FpZNrnEli%2FNEHQ%2Bgi9Esm5l%2FQSsm6QAaGRtwOnOpB%2FzcfmMcXOuN18AItCUbwYpTtzGV5xLcDkCy9GMV3X0ql6JO8IoWdyVM6z26rhYLRqBOvEKax3tFGPDBuji1QYf4zQUKGi6HcMyWJYA3CA7zru%2BslNsibzvF9L%2FJNSp1Hii0z0u38ie5z3akk9b2RnjDOy7rGBjqkAZ9FFm05hDmcqrTLbcHkAoJ6OtuOm5mb9y6JN2FKSnHDSzSXQbJTPK%2BgLRnGtUYndF%2BYGXI%2FfPsVOVJs1%2F%2BupkpvR4azRHSJyF9MSFamDjnMlXfnNfhsw66ZqBU%2FKSZxYuUjTHPlI8quPbxTI3tsS5SecSAj28lN828zuHkDatLW3rdb5R%2F%2BTM9QS2HbB6rKPkaW15kKUYgYDZMWIjRFN2u%2Ft2D2&X-Amz-Signature=fae75f831aff2f0081d65e868d749fd64c3fa4ee9728d507e1c953edaa56b368&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

