---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ADBN5RS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQDtU%2BDg70k9h1vveDG5UJTDdLHM4e%2BocudJTrk3vNF8%2BwIgMSVk9v4%2FHFCtBkK2Lgww3%2BOIxaDTJGIdat%2BUnZmJbEUqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuDUU8fQizuuc5XcyrcA58PlmUgBPvhhxNAu%2BfH%2BHmN3LYRowbHXkEDRmQy%2B%2FZDPPUJ3RMweg1QkyfxjtsQsbMKGnEV8prPL8b%2F%2FmaB4l%2FWk0dCiwoZxMxnLnBH%2F%2FMH3QU8Up1%2F7X2nXr5WZJaeC1gcgfAqLnH4f3AVYVlcFWiZCzZGgAN6OcfAbsgQ8IvfvR7QVMwTa%2BC6BTB5T9bYGhQX5RAJnsQSKP%2BA89E2ISzuKY52UNPFJzRuvpV8ntP5rTQzGT0%2B54DrkzadVo6z5RF89HtGaMh7V7VwgxEaebaQoTSRYzlfPtZ1NU9qdfwCrnMxty41Ge44oh%2FVofmhKhmv71LHV79bn%2BmHR6cGz4iE9DL2Xgi9RVtRTrQB2TZBVxz5H2xynpOMYZAgaZB6T31ieyfVTMzao6VWe3jKgV9c1SwHRcCDBZ7bwxly9etmGtESZQrf3go4uhACAyEy3O9zql6mZkI%2BH4ElcC6lQZN2vrXvb2PCwrKCnuCs9M%2BShYB0aMRTaTwvwZjmTCNbkQcrfLh%2BHFUtN6I%2BY4U0iFjAY%2BCEXojdCWAkpfNBKKibmprsRC55C2hnySaw84ocIl8D%2Fl6M3GK3m0ZhTi8YZayHYBCGNfEoarKeRirohnoiKib4xQsYlANhGO79MPrip8cGOqUBexf091CX1ocIk5erp%2BBAo%2Bg6dK5XTF%2BxGzbinZA%2Fxzof%2BxfKz%2BtU6g8libOXS%2FJymrP%2F6cKkWRv2kL5a63Q7Id62ntnWltYsfnY55lU4%2FF9UcJ1KHtC3L0fwnGUx8eTchT9chCdrOOA%2BAyKsSKJOcNd4Zcfd2w43iv5HSRtjwOsoUMD1y2hy06PDNy7q8oyASpA8bMH493SDErJ0vYkZ8QqJ0KlE&X-Amz-Signature=491ad38578208052388aac3eb7c3060b54cf4b00e64645bb0d144a42435ab65f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

