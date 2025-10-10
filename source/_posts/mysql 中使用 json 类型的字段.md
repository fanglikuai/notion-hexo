---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSSYRMB3%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCKoBQ1KPSsj%2BXQEb8VWOGRnnubsfi3Mw7YG7efj7I%2BigIhAKj2vy6nvfM0rminywuXsVEKkn4z26jddMt3ERj9XX95KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwfcmV3PNOOKTqoUp0q3ANRD3wm0bsfnf8yenmNVIQbBM7RlS09kxyLIUvnvYmbhXhAD9NyE46dbn5FlY%2BvlT8Tgs%2Fgcg57Z7FxIBQhdWqB28qQOEZ9nY78DB%2BBgl7x61BidKJx3c6%2Bv5gvD0K9ImxQuh2%2BLJI6qWe1d1QMu2UJXXlswK7ilT%2FGda3w7Uj%2FvA4V5HZZYc94spLS87VZHqvdFNBZxOCRLd0nl07P4sjFMBSltrSPKLiSCYTPA0%2F48%2FuZSdA%2BoxRBCURbb5a7TfC%2FcSlLNMsnC0n0ov%2F3UWvTBYP1zOpualuay8dsQAdOd%2Fv2kTUTVADF%2B4VBFffa59ymRrglwNp9xlwQXW0LhSmr7BYncq%2FdbXsm%2B930U7aKzO2hDBaILN3dRQwaoo0hwf0N9rALCn2mm5EpY2lILE5%2FplTqbsES%2FlTfHegJPa6sARTI25%2F%2BzZQax4TJecc4btqL2MsbG%2BQu5Z9OUQMNN%2Fl0O6HUgxWMEPoRPi5UeQ6GtktZs0VGPTT3Wbavny0A2cRXqaS%2FUFIWobFDDGwmRiz72ZYwkwNre%2B0g2fsVA08n47MvHz7tj4enmyLWGS1Ra5FkHMI3J4BO2NLB5QUjpEm2hG2BGJYcIQgEPLaG0GsZCADbs0I3lBskoUv4tTC%2BkqHHBjqkAQfPIl0vb%2FDpm8CEDJ5%2BVrZv2Ykr0cNjU%2Fy98y8c4Dk%2BvIvZTwknqb6oMKP8KOWwIypYr2xaRBc%2BA2FRtIMMGvh5hwG1JLrOIZHRuCtQzPx%2FByhvTP6ZgNDwxSVtuB2n9yz63xUPz43xzmasZYCrf8PT82ZbWJbhdW89H%2BHH9psgB0uYGJ6crkGWEL6EEeBq9U9%2BWHNdgGK%2Fsqk%2BxRqP3tuNfWgr&X-Amz-Signature=12b247c7a8d7e10d5eccbdb80d81bb488fe6c7ca1ae705c037df21f43578715e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

