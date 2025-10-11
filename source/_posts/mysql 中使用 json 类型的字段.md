---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3P3N2YX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJGMEQCIDFBxBnRBlIb%2Fa0dAjevNHImZQzGpzn7aF1385Sl8khlAiBNdFPQ25XIwtGi28YMUeAjEc6z3%2BdUY0xSznr722E6zSr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM9gAn79qFL0RCcYvaKtwDuCoPM3uCD6fxQ1qM7oUOQCIdMfqNqzWKejvw6g%2B2gUd5k3t3SvMbZ9P4jr12rrMUQx%2Fp%2FVr8MLV%2FT7LEp35Fngs58Q7djSkpwcFbgvwFimzBpngvxlvKUFehNiA19q1VTstEGgdgyxLYh8GuiH2JwHXLiCMytKFiphlWnz1IOMAtXwknl%2FvCUzHokXRAXCXFBF%2B0IXmkB0yDqnVfOtVhgkNKMPQXExbREjlo5jnd6V4qxprzD0sqOjfF8vYO5uM%2Fbi2liAzP6o8JrkZ48m5lFcFj57uPalwyzSNWvxz2HtDAuMAadGT%2BZrWaebzzy3Y5XUOnDPCkvG%2FprjXdbi4fOJefGbx8Tiz3exsnJIPC%2FPhCqdTWSUvYk5DtYhKSKcsIvvmIK2gNTRtQk9p19raLCdn%2BnDkRLhr0PNalOoZkF4vgo7P9jmbn0xtAlGvf7D0%2B7oGg7jlZQYQeR2F%2FlDfi29YI7kZuoqa11UL4oyNClyBO%2B7wtGIkcDO7Ct7%2B3UBs8AbA65YlV%2BfhCZiLbOFyp%2Bfxmds8pL3I3YDQGIk8lFQkmXgq5VAlgBdstGydbJZCyrn3hQ%2BxZIQ23HdcP3xRHp2tvfvXZ7BejgJH%2FgVnKy9l6owODKczDU8XRedsw%2F6SpxwY6pgHq5mIes30FAuVkrQFnknBo8azKan4c85JwHsguCMeEQ2vOhrIrxp1Kju0%2FxhVlqat9OT9nYXX%2Fj60uTIaUwhll3bow1YiKmm%2F9gWa3apSyO2s02W0MZ6lR8soi3kSJddZkxBPo3KeorHVXJq4Zx1%2B1u04R22KqqCeLB9e23PU%2FFM18ZIJNc5mtfBgfB4DEBxI0TO740Lh4ZcN%2BfI4CKq10dNboO87T&X-Amz-Signature=ac23eabb4bd3ff7d33f62b2c4631d7a22efb8e36f00b4fa83b6c4a43d25e0614&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

