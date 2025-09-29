---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46656PNBO32%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEcaCXVzLXdlc3QtMiJGMEQCIBK%2BL6MMsFrWDsvCO1JDB%2FLPh%2FURdebWCmP5e4sT1L7EAiB3kV9%2BAYLnVM19h07waZ0UUUQMtPjhKQWPWho8GGpmhyqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyqC4rLvERAL4KuziKtwDoHWqPdvrEJNBnNBmJ0GPDLz0nafVJCqmCYae5nR%2FTcQVb%2F0YyL9570VgVGGIZX1V1KG3nZnu3i1pRKvFkmuuOkuaPEvEzitUNp5kiapSlaseRCHwBSy6yDCXpW9daea5%2F5n%2Bf4l0xypgp7a6NG7cag32Olb%2FQ9AdpC5PtgAoQtCTfKgce6HCdRKPnyvHjUTbytHBr4zAU8clrXIUzw7RwEs3z12gumBXLMZlPVQkPOx7OEujBRYiwlQA6NrL6xHLmcZg5bufcCWxa%2FU3L73Vmvy6kTuChBsOt2PsAqybqbKqN43zjn2YX6Mlz0gKyKyMYzyYWLr72h20aVmFztXFUM6KWh1vfhtlXgsM2baVKGJBgOJ32vcq73O6SdUSKgeyUkRQLyahS%2FcqbmnUgSWU0MKM1JoEqdbFZiLmp2Vc%2Fvg0xfQj%2BxduqM3O8Vdk1ky36RLMa5IP1IEbaOeBf888FvU1KKyCAz1%2B8YrX3vwzSzL0xCclGj6jGhGCSrCvOb2i1M61NyUoyOSnWdcB%2FR%2Be8dyKI3Gins%2BqCoSBy2PsOP%2B37z%2BK2mssXVwVkINKnTfLd7p%2BBnH%2F%2FRkg1al8ij79N5f8%2BKAZvEfEiX3vSdtXSRJy5m8c%2B8odVXun7Ncwu%2BLoxgY6pgH%2FtY%2B6wxthb0h94QjGZ8Q9in8tQIslm%2Fv51HZZEs%2BXpqCu5iDkRRa8hPxHF4%2FqVdTWaaQePxYc0WqHmztMfD3BzqHkS8NKMXE8eX%2BKhMKUnuVntb%2B%2BVUwK8u7AOlFIHh5PKf93ZD7e87tou1Av6HSxBBZLkZmPCry%2Fhpcc24LCy9pKn9TKROc6J%2BNj4MUHBC6G29lrvAkCEGFx9TBBtnTaNhJ3gjM8&X-Amz-Signature=0e9f9191627a919f4aa153e05e5abf62c9a5272ccf1bce197f823783c89779d3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

