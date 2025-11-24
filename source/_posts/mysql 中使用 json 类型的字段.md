---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XK7BD64H%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T080053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD5gPTORWipV7vOqa4k9%2BofArT7jMM3IEkR6woyxKcXhwIhAO9kAIuL2eWAawFjnnaCJTFZoq%2Fjjih9gOyjovbesnBnKv8DCFAQABoMNjM3NDIzMTgzODA1Igw93r7MGCpqm9yR5lcq3AOxpIv37UUGhrirkvSbG9zmLstyifO2Th7AsoRDKzO86OsX3PIZvU8AM4x9F1MHaNYEZY3spb26x6Lk7UyX%2BXituCpTz2LRtvKG2AIl1Hohf07v1evg1tfW2DLlIyz4DeSLNP24wZeG6ovJdIqjqd66gmV06CIW1r1nMzZQUsEYMJNzcGc%2B898MgRHnghyeA3B09Vhiq9WUpVewLGnCKe6YJ0JLy0suY%2B466jNWEEk5hS3NvtpElz4SzOx19K5U9lfO9xkd6NxutsjLeVSBXxJcVRgfF7AHisq0N%2FUL1wxpPMimRp0tgysVR395VOSdK12hNDE4r3s0TDNgaIxa4nJkNP4T%2ByEpJ9PI3FMRTPWhKMqPsYuAyHnkSH%2FKDD5y%2FXi01RnEuYtezaMDGHwepuPrGKWVmCGUJRnkelX2N6CnQqOKOS7UCeHj194uVTqDEv6ztpc%2BP4lNqNBQXAWCEnxCpoXWIi3G%2Ft%2BYSJstuw25orzu0UEpFbfQea%2FE3eysBHKtn4zqp1OaGrKtvVP577hes7CnhGLSLwyUBaZY%2FHoAkgjkJc%2FEsjxtGGFTNdDUmUM%2BB7UDgmWPoDsS0fu1qHkIJoHQ8NKLebNaid9DZMlilvc7SFQq%2B0TePEOYPDDxhJDJBjqkAYBM2AJRweHUnnjPP3dBGvsvEGAw6o0z758lxNGUmoSUSrh0jYSlpJXz00uasId5hqdq1GYDHjy3wVhjUqqtsmsujFTIbD5auMBi6kuF49RzfPgdm4D3FACvKscfk3LrW48%2B07Dao4AscnnXSztXRtBiQdkIq3tBwjbluDH3w3UgeQch%2BQuiXuSz3wq3mQl8SVWh8d702urJHF55YBlpSamiDWvp&X-Amz-Signature=2d2810cd70e97f04902ce8529b45d0b59e1b7cc5a4150fa5402bd241c3854ee6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

