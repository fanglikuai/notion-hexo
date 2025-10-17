---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46676U2FF4P%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T090057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG0ZHP6C0%2Fm2ljowRekoozTN5O450gMwkf8X3c9afB5qAiB9%2BaZHSvGyCsvtnLNeJqpWVp6J1Zb%2Fb4eHTuCjAvu0WSqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSGzxBVIPv8rYxH5tKtwDrXJMBYBRFZbOO2BezbhDwd6QUPIMIyruXm5uR%2BsDSqr%2FQ5n%2B6mfZ%2F%2FsCHZU7skTBSArnDiTPt4twP3yRv1Ut4YJgHeR%2BkbRgSD%2B9uA3dCWKGDTUi%2B06ou%2FckxqywGg7UjeQhBMr8%2FpKH9JtDlIary43m3F6PAgl%2FjPkxAJNU8PDDzP20gL7YdYZvhEzDxcTUCzZHWVqDqMr23X5kElcJct%2FGTA7PMpTXukvCFN0ny2qEGieAs3DkxFrSO%2Bqtg50gPFXuB0qF9wfK45bdaOpEOMrOlAjB%2BWqy1Y7G5PBrY5SWKAvhHVMC33l2FdRdEVRKFiEprc8WvUr6A9KqkV0476ZSsT5A%2FS40iH1q2SygxAyAGg7CEV065JYkDfb2ehKboLZvZPNqLrTGXZ2da2hxPkWucvRya8IEe6ibr44mR3S4I%2BoTKHCjwfupuf45E%2FoAibJN1TCS9Q0yn01iFTgewvDast7xt3TlfIg4viyinER4TKaZZiDiHOSn4L42hF%2F2MJf7G7GiFLq4DF6el7pEIAP8Nc3NxX9%2BT%2FaoloccmGDcCPsifqei7VTSbip6AgV1b7%2FhBfQnLEmt00XLeGSM3iMgEDFlxBreMTnGt1lzH3azSCG1gSEtQJbY9OowxejHxwY6pgFZm2Ocev2bBL3I%2BrZuOaYj2yGNKjEuuN1TR08rUyeG8mR2%2FbTEhE9i%2FnzOlfiCkRFWhtD%2FTObmp2NXa5whGsaiWUbiNGN7PidWiLL71BCEdpU6EIisD%2Fxqb11dQf6ZbNj87A5OFfce85qAg2FkmXoAwHds3DgFia%2Bsu8mVcnEMuQ40q7yUOhp3Uhr%2FnSIZh4gfFaTk0tVCoeh8FvMdApvNWX73lNVx&X-Amz-Signature=317bc62ce4e95446bd8cfc828451cedb8497684903af891f4e194fe9babd1031&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

