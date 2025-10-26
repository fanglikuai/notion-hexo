---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQVDGUC3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAarK9EH1yp8X%2Ba%2FDeNZCB0fEfrNNb42WPqGmBHTEVBRAiEA8jM0Zh2O7OH%2FPRs7zt23EedPpI7HM%2BrMzhnAiam0UKsqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAts3fPJ5yF9H9zFhCrcA3hEB14SJmXG6IdknuwP8oDudNH4wmECw3eNPu%2FsUcjL%2Bb2Ue4Ma9JxsrpSq9fllNWnjyiFP0TOaUhm85qbX6pIfNV%2F085LQWLLjlv1qICRddj6HuZVEPZOi7bX2uPcQZ%2BLohDkNhq1IwXBFzPKCf4dNriGzxdyjPkelj9fN1E50kcBTUWsGJCfAnTy89ukwI7uEtNSEMwnj4%2Bs3Oo5HrYAa5nW%2B7O3DMKvK8gYsJWdY79wUO%2BHRRCSa26YI7KCJmlJi9Yoc5oQ%2BmnJzt4ifbmQpB%2BefLcK21sat9xlYsb3dGwIbS7UO3xPJYCfergvAogvSXvgelIVLB6s99WpknLoU5ZapC64wW1uMbZtS8pWqaggJm9o0v8M5Mm%2B8zSTfgqQiiDYAKYWMMl%2Fr9dKm7VqQeIDVRVpBt0Ir9o3Q1M%2FKlFgcEgSN9G%2B5DNWOPPODdOM4LnmfhfVUNoargcTLfbmY6%2Fpy3LKhMfIIvF%2FwoqeBWHLHXsfgKwhE73X9qcT5zEVhByLU2qEbD8WatIVPL2waw1h%2BDlcTaCB0lG5mCLV28hDyqcl3ZtiTvHecTYEIFDWO1bPUthWVPjfwI7R69%2BinGBJLpM88NGl%2BmOGDM8C5y8FOcjyk4rA4o6qhMMH%2F9scGOqUBZgKl%2B8Aj7UpGQ%2BBM0njwPdioaEcp1AgNv%2Bgdppba7zoB1XPFKIxmMc0tGMICM8CqGbxGUBTQjlIM%2F2gZTzqSZWH%2FQTCtfxaM1ZfnJ58JaatsVWi5H7i9x6AJRN5g%2FcgnTq7R8f1hez6CsihWets29yFHzry95gyjz6Sz3Iwnep2xJU8UAqs63xH%2FxfFTT6M34DNOM7MXHcFRJXlywVMbpbxueG%2Bb&X-Amz-Signature=a2d8f4d6a75fc9f1b89f4bab138423cca7d9d5ec22564cbc3a181b5f79aab274&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

