---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RW2V5KLL%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF8HkDSDsg4MBjAjemPOmPMZa3lfxgCg%2FHKziEFEoEUgAiAVhwzaJ3v%2BpMs0sp3K8oUU54X0BCOOcvitN1rGBOxAqir%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMePCxEz5bGRqejgzMKtwDPuX9bpJMF%2Fp2ypDYYZJ4z5SX3muru8xmZMUDlOLTmloViOaRkHUFhUbPPFMP8AsgmssSetwdNZDdybZqScwv7COPtf2pyZ%2BVdOx3lugiEbGZMyLapf7usUZzEWwljzAPivON3cM6iS%2Bl9EpbH6sts84yNVFM6EyJtkRamT2GyYQ0tkuVrjK4hMCyj6gvyZ6RHV2Z33Bqv4REst4athBeLtBRR9DsdupzN4IaF%2Fm9QBI9xAP0ytr1WVi7%2BsNh3mf1fHCua1srd%2BgR5AgalJNWzvpnfaNNJTIvICreYlQF6a4Qw2Szx5xYyZIgNYl%2BrSrSbgruBMFX2NxUHvUawwedIoVES4xgX9ctvxOk0PammRqctrm9rmR0AQIMYYGloCAuVJU8EL7W6emFQHdYga5HYc93umDPXLAtlPgOEgNrDAieaj%2B9ZqKtCIrWg4Oc0pjoafDb6my6qOGJhnG1E%2BNEBWOfwfUkZySMAN95inEsbDRplob75hq0zHFEfscukcXnpydpf1dcfH%2Fx61M60dBabXp%2BZqjmWdi1YCfqez3ysGvT8pMHQRR0UyV4CEmj%2Fu4QUdv4z44P1xfAgDsQ4iuHj1Tti6VEBjfA6dys7OYyf4jvQBBYLt95rXcClCMw8tbvxwY6pgFQpWNQzZBr5cA2YVkjiZgcANZ6%2BMKBEgvTYw%2FtijbUbtrtGa4HtzEMP%2Ff2TTiwmelw41pqm3SdgE%2BJbTQ5K0V%2FYcOKkTim9%2Fququ%2Fnb15wVPupNgB6gf0%2BFtrA0SdzSKVRvYC4o28gO%2B3RU9nyjBxRz2oytH3AWK2AP5SjcoxodMuSPhR4EXmLY3ivm756sG4pDeo0J5QWkJaT6rx9H0Rx4pCEw7hT&X-Amz-Signature=2f94f85e2024672a072fa518b8481e8b876e9fcad857fdc0d601b2641e9d22f7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

