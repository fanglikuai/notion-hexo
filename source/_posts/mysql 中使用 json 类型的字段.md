---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VH4BCBCO%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCICJrBiz%2Blfj4oID3ymn4glZ7bIYh5o9zaHxeKvS1piyDAiEA9b4hW5TvXTJzuQZ4K%2BGRkn6Bl5uOkrPdt4jW59b7n8YqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BKNZQ%2BpOIVx4TlEyrcA24FGF3OpDbAo3Bok8g56yQb1FD0iQAvKXtvXhLw7eA223RG%2FJwXFoyEsqcn9vaWPd63vYcdGFKgIPkkRjkkdobtHV4fa5aNflCQFz9woVarKAdMztatTvhe3Kg8YI%2BIca8u1k86kRDYI4%2FS7KIhILSAyKTKTJv65HSwH%2FoF1WUU%2FFuE2HwzCaCUjGF%2FukziBNuz4w4GF9eVcEPcKVsm8GYJUFv4Yg57IM5ZfN2VLuKKuq3m33NAAK%2Btn8XTBzoZ7YGieMDes2kmQLubzGIQq8umuJBeHWBLweJgs8MU9QwiPWL%2B2tCWYfAl3PGsxFv5sFQneagLH8B79FBa1xRbC%2FqQtoQdJsXCAsNgZCKOXLUbDDHMr5UajybKqLOVj06led4Z7YCveK8yeVbZRG9PiSSNIACFXfMtkNlf7d5EGVOoiX%2BwF0XANQ1YPFmvQUNCQ%2ByRZo8VZ4nXCk6JcjPQd97MWCk%2B0eK%2B5bMhMf2%2BYY0OYxbpC4WxZP%2FYhvYG9e%2FGFLf5izMQOZckpp7QHN95PtcINr1f4bAJT9i3ySv9xM6kzxM0pCvHLUIg7OCpFfq24yiW9dTF%2B2dYEtiwLntIRnCzG1gDG45mCneuOc82fmY%2BbCMepGQ7H30et7iHMO%2B008cGOqUB5Af%2BmYPIjxY0npj9ztoV0%2BxciIzysqXKMTO818kZycLqvnQK1nLT2sA20a3TaoYXHVtZ92rsf4jdJUETEZCYdmypwurmfwBJ4PUfBrYr2CK9%2FV6hXcEF%2BciOjLTRYUrys33rEJxUaR2UF211TDoXcSzneveA4%2BDeF5UCGEqRQzWBd6Kg7jQsJ7iVJpi17%2B1OFTiWq37qjDAyaLuMCEKrE7aoGTBB&X-Amz-Signature=9a22fb27e4f1547037f00acf85841eb1fb71b1ed19cfa96df852f7eec902f0c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

