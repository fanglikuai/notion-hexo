---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677K5X3FP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQCjpyeEOm2PenfHuzYyWn2EIAlIhVtY4r%2B%2F2JaBHUjAtwIhAN9lmLfSzDcijHrRt3M5DRQKFJWX%2B2pFYP26kXqBsOsXKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnkS5M2ajd%2F%2BxqqNoq3AN2OjknvQhuTXfoymWXdODXvSEjD0QzUqi%2BwHqUbWwkIKyyEWdkzyp5aZXctqAxbkrR44kYFx9IGNPB5hh5ZHosxevVdqtU4D8e3KAp%2F4IqO0eNxGxPES9Ry%2BIpW1jI%2By568YcIN28ardfJN9INkZogZaxM8MuzJacTYtFoISvMdYYPdQ0slglSCFeXYdSbmmJX4FNq%2B54ck1GYgmsReu%2BskGlOs%2F%2FtPcMzQqkER%2BRcpmSFOe7ZSbJidv8XcBTdl%2BWLkf%2FF81QOLpBMCDkySlvU%2FcNuIjt5dnC%2FbqIyLOZJjkL%2BAtgCCPQINU58LvUx%2BrwuKmQl8aA0Rxy7078od27KVpbSA0bOgDblp5LDAKxEtnHgiKVyQkrB%2FC46zty%2BwvTqtRZ2JAmWdZDulxFdtIBUw1Pf8fADucwZuc0P2aDp63Abp7dmPug%2BGjPEfCFGOq672GDfygbS%2BnB2aRxjs6bsRZKcUSbZGW%2FN7CJihrHcx8xDd%2B7w05so1NBI0SZjz%2BZVhQ85t5HMkDTCW7fn9olCUc4BIbb0Vh56XyfHSgjIEkEOS2oESqkUf7QT48Lm%2BxaJLvtfIkcskey3f1Wfwo6G8XEe1VcCVB16vvH%2FDSM1SAvm8KGM3PLKQkXIGjDi9ozIBjqkAW0iZhzj%2B7Ci0WyU1BL3nhGvptIaOQFnLMHCacx0fgn29Mr36yTUak8Psk%2FzF5%2Bilnhj%2FPs%2F4xQE%2BnVW3W%2BE2atgkCsV6mUMYifUTPLt0AjbI6hLBYio%2FkEE4lvo%2FtYdUmVKW541b5bTZsYQq1U%2BqhiQkbX%2FoqkkJo2vmjLrCJkoxTCK%2Fdfo1mY45ejCcJesK88ekSWDpgIKUDdw20HOVnGQNoJK&X-Amz-Signature=9247b87244529975fb4861a61436248be6f54266423a880158cc67ccc65808ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

