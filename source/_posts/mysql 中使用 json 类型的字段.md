---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SH5FXBX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIGyF8wiIgtP2eXTv8AR6KqLyMXTH11%2FXTowe8AQTVQeSAiEAotazN1zaHHB4aK75OplWE9XnMG9xE6VnyBI33Cuji9kq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDLomcy1EAL%2BsURKJlCrcA5kVtXhYIFFOpcnzzdV0%2Fbi9M8EDdAztKLpgBKTZRk6nFt5reb3HK5V5HqG78THWK8oIrr9Sz9LtynsR5TLO29UDWXkeb9QlC8IzRYCuTuIUFL6uFGHPkSaGdX%2BOind0AMKA%2FB2IvycVlz1VVgMxmDiuVwuIXSARAR1H6rn3jULzejcgY%2FTST7xBz%2FRYw3cxkc3%2B0PmWIaLnnje8PQXOKLwfGbF6iOg7S7X4AK464QirdSrXnO9GwbKfSUt0x6ekpDI6Vsej6O99tw8yu7QgO1MN%2Bw%2FgSoRNoZSVKP2w2Gb1NH%2Bp8P29ZV6EI56QbEK6K3mVp484jHH5W7iOEJlGjrKD2bLDQC%2Bx494zktOIH4PI%2F4NGoEhJMVSOkS4nPgIuj3ylT2lOYsZlZU46iBWk3PZ65OV8M%2FSxKcRDQufud%2FSdNCK4cmL%2FwuplZFJo1zHtkMuvKNzJIBj8mxxXgxA6fT6sFwv81VofOt%2Flzm8XBdE8kN9GeIhUQDISuLTmnlilMn1xKRbxld1XdNV9QCcfeUSSWJYm8Kp2LZLcxsAUjEQe1ygtL%2B4Fk5zQRjqOPAIyHLFNnsX1oWbfmGf66BHG%2BjjkM0TM%2BspUgQCpqQjYVWhjDzup4ymkbDC3ikAiMN7JqscGOqUB2uSNB2UQEYyMpiC7QFbDY%2Bol4qzWbZnq24QCtn9PSdXX0GkE3OdfP8xXXBTkT4eVLbMVi3XmH1qrJDNjWg7umJiz1jOtomO7%2FBGGbmrfD01iVYHD66FpiZSSLz6WCGMW2TvypUrHX1HwB6hgvZYNjB%2FuzUGPGBS8NzZQTxjHXTbbssRVzN250AXCGP4P3%2F1rn2GiEOJdYFxoGQ5yiFKozE1Ey3so&X-Amz-Signature=6c23946d6a8a382b23c41098f82cc8d323b01229abdaa1716a00ac2472dbf2cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

