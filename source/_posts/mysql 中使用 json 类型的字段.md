---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNRQKI6K%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIFjp%2Bic6SNt65bCR0DaME0DS5fvVEJPXNyzSinIA4VDRAiEAm3rdr9S%2FkBrQ%2B40HY%2FGWpeHPq977btqyhjRelCPh5wAq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDB6OTvD9%2BrFIO1DW4SrcA12OXcrs06fQ%2BztJMyvnZ3FwOcmY%2Bvzk3bl9v8kTFHgw5OsmDsqCf%2FZunQTpN8Pvbg6nPxs4OwYJR249FI2CnUkg5GoNBR4%2Bd8wXBExTlt5XPGqgsCIdUTaPwkzIrLEBjkpKTH4CZh5BghS%2FivN%2B83mFHRnsYIIlvoSNS25GkG8ORHvtw4l%2BFLVEn9fIlIhtZyv1mHKrh8zke1CxXSe0%2B%2FlbmJtwoASRDKkU0ZMws7KuGUW8MPu7QmRVVWApKf0imEgTRxMYZiWbtezkWMPxsumvGaBcBO%2BQlEIRh8wW1BBQba1yLZ3zq0LWcwsU2gtM%2BO15w5Q3ij%2B10S9AmR3jjEuHo4kmLNJW7r1C19mYay5%2FU6gOc%2BWKMf2qOAqbZuom8jf1FnHcPRTBE8a9KZZc3Kc1lnLTF6T8sh%2FtTzcRtc3M%2BDqAUSaPEbOY13a2saOfIgNVA71MI5LsmYl9MOZ8mm30GWN0PRk5Og5E3ddLwddxMIZzs8OHk2L%2FTmdO6KLc7wGBqT2ogKJ1VRUVM3XOwREV1JgwJ%2FwKZ6f4oXyiqKuAoce8ur9BRwyIKENsHod4Z83mEPlOTJ5wh7NBQaiRTrNlRluevBfuAFPQsYm0MVEifYgQlpphyvzAK2bbMI%2F%2F08gGOqUBoUVHejn144w4pQ2qqYMv1QsyTnNnhJm%2Fig2dQk9U778bf7u%2BnCafNi%2BOJNSr9TuPZz2OT6hCBoH4oPGhNLEe6246oHPhsD52kmdCf7kDU3hcQVTZF%2FCxIMflOwSDaUUugJU2hqtXFb3RWvg53skq4JOuViUj%2Fs9yTwhOH%2FA5142wuUrDTW918zYMw9fX0pphMcFj%2FNj2%2FmxOHI9kwWWMRuj1pkYK&X-Amz-Signature=5fc452c883216e2fd2875ac5c294c585549793c422fdbada18830df1d00f67bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

