---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HLPMO3H%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2BXuAC%2F0s8uyHkkKYjU93Jg73M70FD0BcnbSrCw%2B6VwQIgZhSyUNrn8LQ8u24siqbw0vkGMPbt0WSYhyGZdANPoA4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNfFgCvm9ODFQMJ1eSrcA%2FGquoeCeeGSr97u5T3ZjhsOelijCcrI98HvRLWKoaSqqjoRHK18If95MCWnD7NFdIwmkYOBzLx0eraiXh089rrLoBhrncG5sE2doATHWdeDvZ%2BQFW1gUWiBspDfp0M0H%2FYzeGLVi6M%2B%2Bw8ueOBq4Sy28mOmSldft%2FdlJATD0XfrRBhthg44aCcL2dNJuaQNOdtq%2FfNE97x0R7OpQrnMiz9Lw5Us4no%2B73E60%2FSTjigiZoxknX5l9VJIOIfcoa9NkP2qNBExgzVAUEFoDu%2BGK0rczd7dB9NHVP80Aj7X%2Fdunm%2BtbIpv8i0i91c1FkydpknBPQLRCttXmTd94l6XjHuwR9xZT5xNSfxNIYX0p4ct1g7tPf3XdfcGEOLbKpmzRZH6EVg5nj5K%2FF5YwM5MLRaCbxPM6WzOkVX2trgt4Rkfksyd%2BZIlbXdvz%2BKixMFXeZ5EKv3QN5gW7PDa6W%2FNlhgOSuysQUBZVC7ZCBEAmsxvcChwGs6sQmmEoOpS%2BX90ClRihXxhcCC19ey5h6xatirHEmbGgCRoSDgOoAZ2aI2ntbYsde5Hzq%2Fki%2FxTLesF%2F2%2BukvabHexBYQlpnEp0zm2btL1y%2BvR%2BEX49A6vu%2F%2FEFe4bq5aBNSkI%2B2NBEQMOKZ%2F8cGOqUByYToi2fiNnvvi2IDU80GJ47dGK0LCdlKQ1HrzS1GGXHoXTKQ9%2F1i4kOSkpk9mqCjhl8WYFs9KZeG5IZKOzw3Fc306YDByIQqS3jxWv27I6R4ahoTuh%2Bo5INQm3jidCXuWIHX6R66cOmyQCvAUGE9pGHzGXLRetK97axY0wJMzcKrB3Kxox%2BN3bQeVR4GeoI26AXa1t6UhR6z0m6BxGtwM1QMa6op&X-Amz-Signature=f1b8c0ad3bf141bc49f962babbaf5f01f932d536967991c7aa5335abb0239c3f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

