---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZDDUVIP4%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T090040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID7bhHLPeh4W8XO02GXuhhlkHihxISYhblblvjNT1mKDAiAWz8byUFajbe3fp5szNOYzkaQr1pIDjDNOEyTvQKbvlSqIBAiK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqwf3E5qGjyjBr6m2KtwD1LkDUmXGraNZF4uBbxV4w9z0nVXd8W4BoaJ4ZRXGVMDMy7P6S90EEMea8XaQ%2FA2LVDaFc5qXJi6ovMD5YSFPLFxUejD3y%2FlYqgjdbH8SFRyc6Rz6SwfCWqzzw35owSeCRUT6s%2FiTzdRxCXob9egvKPZGhkSaagGd5RDfYafUo4xxM3GBJzW7cVj61fO8%2BxDRhDdWa%2B541OfENiIKdt7vbtkhJ%2F3gDBC%2FkTrOCOa0q5VueLCtBPtLuT571%2FaNg3Bq2GDFSCDtcmRLcuhceFjpWC%2B2w%2BQEkQJpCxcuBZ1r19mh2%2BNx5xpsOydXUzbHwP54H3OeeNQSqL%2F6U1C9mPFrHKL6bxz4DHZ1FE9cBKIWwiGOoy%2BRwBmD%2FXI4Wn%2FHavwXXgg7abEd0B455%2F97M2k6Re2HlY19N9vlkP2oYx66rBrhBFFnK7xPnQZVBmi9ANee09SymCSGy28R2xae02UPPUpFxqeBKFktJwDvWtyiXiC8K5syCl3nsE%2FQD2Chmn52Fr13G1emYs3cyMkvKFermA7HCeIM0O9QajB2T8y2K%2Fbb6%2F2IuC805KGvixP5hj5CkcKR%2FyeMnUlbvfJiHfd%2Fd%2FLvPcz418VBwERCyUawON6pEPjBtQP7tYBPyngw2dnCxwY6pgFyCpDF%2FNURB%2BS3Ogqude%2BUgE6eMMn0pCfSPY2mXyP7ISW3RpWF9evMuPtmIwzRbkbsB7J4RiIrvJH0yKsvkLDwv4nyiKP9TVbp23eDPkDBc8lG9KptZYqyAPq%2BnP%2F%2BTFMy4tYn%2FhAxQW0aBufGXH5TMzRfyyFdnOS6w3d59cfKHN1JDz3Fa4tts0rcKNgNTt022M9jg%2FoTCfeNKKS%2BhlZVW31zRaW5&X-Amz-Signature=bb7f5ddca981b6448f165aecb3e5a0be080b57b09029b30bbc18b29dc38838c9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

