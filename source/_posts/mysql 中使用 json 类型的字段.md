---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ6OOA64%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJIMEYCIQDd1sGhjf%2FUlujCUEDjOyyqZfFOmHaguxGTKeAyKhawOwIhAMb%2FydYA%2Fs%2BJSaTfo4hArtea5hWnTiOHFWyCpZvLxPpPKogECNT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxA5DL7ytMg8UL%2FQbIq3ANmSpeRAVQ%2BrsJj3FYcWCiGdY3ce73dbk2itohfLLY7bUE9du%2F%2B6lgdq%2Bls7qCUYCCvaGsQikSOTB8Y11Gs7%2B67ZcqIds6o2ODDEAcuBXr6%2FJYnDtBqh0al6Rbfhy4JH%2BpEMkbYRDXOMDOACR1AgPoAYWfsZyxzdDLJd%2BWwGNGVNEq%2BpYPG5QV6J99b2m%2F1U3GBBjHogWCKjYnawRWY17dlRQM1QpZTAIiiCuuD6kCDj%2BzZO%2Fkjrw7nAnA71avC5jADitzy073aBA49WTRmJjh5pH0Odw44lixISAD3HfVTYEXDY%2F2CwwQno8DsnmVw7y%2FAl7LGqJ55x6QmM5q%2BQfpRX0VGWO%2B08ZHxs79UlU4T1JxfW2GWQk1Opa8G0BY0dRUP9BZcrd38pfRxpQi8wdy7zbScExhxEZdA%2FasVtEl%2F2QLamc3L1CVH%2B3sZvzaXhi3magH6BwVFgolQk9Wr9DUzyXRdgMt4LzAwD3t4VPORhpOgaEDvPHq9IWXCddNaqLjgBlg3Jp3E4gPVD4bu03n8u6DrIg80B8II5j2ttm12%2BCW5JWftIyMQNmBlxm2%2BNgry8lEOgwz1Lii0xgHMmF4exy9m%2F7cRsxoPMVD1w0daBLd53LhtgOlIq1OGdzCz1unGBjqkAQbab4%2Fuw1lZXfcc7BeciT63zX69F%2BtlToF6H2OyiXewtLBB%2FgDfqP0sbskmTC7of%2FxWiniberpdMuAPWyQxYFZ2SBXXF%2FjRu3cUMcdeuWdH%2FXV5EWyrS%2Fyu1aGA%2FUbjURW%2BxzIkkCmRyl1DFfhFh62fPLbT3ppkS%2BWylcH3bq96brl5OrIGSNS7UDXGvR4wHLc0Eknpbs2f944JUMdSczet%2FOD5&X-Amz-Signature=8dc8197d2ac2deec2c6d0756e9274c2ab0888598cf8b54300ee4ddf789eea18d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

