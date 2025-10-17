---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SPXY2UUL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCIHcLPlzo37sDsIXsgnZMqhEoAE8K7oiWx%2FO8goAqdoQIgB4BHA8HtcXgzGpayexBtlbYeljjpBkCpQZJiyDYuT2EqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO2vfVj6XA4QFOcOYyrcAwubzM68A8UiK%2Ba%2BcGnyq20kXhfAED%2Fd9CvmW%2FudSyrqukLZpjoY57yejFNyw0dN1MxmcPCcQFeGGYxvoUrX%2F6hfMkt6aEN%2Bkuol3cdIBUeVVrHdRtZcgTlulH94WoTGEpvEV6fiz97cquuEGoMrA4JvHOZ468yRWlD0h9E5XpevSpYsmwg5nW6zZsptO%2BfZWc%2FIWbx5qyUsF3bhmKm1X6K%2F5aQFD7T2f7RDM77rPaEx2Msw6lBLEDKECTp%2BNkxRs5f7K0qIob42S%2BWVC%2FBMfAs8tnZtnhsp7OaO0NRXP589zwyXnQcI5%2B9N4LqVAjCJiFexXjAbtcHCksKsBLs027XST3k94NlkIPnvVCNvTDMHIBBo3vcEQLznDtHpHA3R69fVDKcha8e1FXLlUA7cbSTSNp6ar5rhfQ34KRRpaqL4DvQN230J5%2BJJT6hr2m66qqP%2FoQFiL6iPO3%2F35JCH7H1Rn0ahn3sx5zNmZk%2FZpBy37mey3EZuVQgB65LxIik4EBvqXeWOT83d7jgnVtfwyU5rw7loDXDEpfnyI%2BSG8hTVEwAF2wVAJ%2BIsNg2JvHkTEHKVIdn7SXsc4rI244DfFChvhWTSwGmyPVOvXrrxTgve9JGy%2BFSbr66UmgoMMMDBxscGOqUBkROLlDZH2mjdLQ62NimNC1w%2FHAFnUCtTKezpULT8sNXvdFT72tPvPgfG5WCCrVh1XFtSnO%2F050%2BX1DtvkePHLIdQwe6L15qUpIXVs%2BoPv8u2Xredck%2Fa9XIIuIEEmOs7lLY5CdSJQNZZUAr6Q6wD6Zhhqa8nuUFjmdVKM05Ehh3uAO11waPSgcsi9woH5m7MhkAehljIwWTpN5pvLa0DrpwivPVR&X-Amz-Signature=58eb30325db1c1c4f884a464b5526f8e2656360fd90c8bb1680da7be1fb96d67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

