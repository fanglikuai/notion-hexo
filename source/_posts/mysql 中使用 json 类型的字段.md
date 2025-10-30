---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RWNSSNI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJIMEYCIQCbwOebCI9lDMbaOM%2BsxF%2BBCuswTvtaDME0rCiM9Sq6gQIhAOc%2FhVNMR6UR0YuCvDSae%2BM%2BQTGnhDGFHc26Jx1fNmdPKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXb5fNBvbQUAMxV2Aq3AOcU4o79PxMWXrB%2BLXO%2Fv5%2FEp2Xrg%2Bi8sBb45cgiyiQhZA9IyKL1GehSc6eD%2F644bR0s%2FveZyE9NbEKRyPfjC8Ou3mpXDx3nzFK66aWYOYbWU9Egm1xnzR3LF6Vsb%2Fh9ftdcEzsXj6Hr7P%2F9DlG9JNvsgQ%2FnHC9ZNBuiRTomNxcITChlFe7eIPDgEOHVi%2BfUsQh7VC1j8KIolaBdul%2F%2F%2FtF3W%2B5rM8yLAaFkqtZ9DP9XDKWOnYHZLFkok7QzJcCltFnEPnCuMuqBufd47saTMqIV3OiEZh4a1OX%2FSHZlZoeGtO8AArXkDVZ1d3unA38sGlp2%2F3FBNBsc3Lf6Y5N0akM3%2Fcb%2Fu1cV840DtLz3zoQ9Kf%2BCYRDEfRuz%2BBYp9R5whwRYILgnLUvfQM67RF2o0kTIju33zBca77UDJTaGgdw6X4TNR35n%2FW5LbtnqAwjbFkzzStQwNaG4gK2S0dsMQNHIZmnQoQ%2BOxxhDlaqinAp%2FpNxtI%2F0oQmslVOOPUWr1GEZp8zIfwHDY9KvsVakXIWDqY1rOmdc8wKAhd7C59X7I1rrO5Dhs%2FVu6LuuyOQASTr8H0EmTL%2FX87LuN3zp7Vro7NBAFvwdTzVKKkWHwsT3%2F2lQY25ZYrRNbylGfzCRlY3IBjqkAT05YUkTSOpimkjp0JErVn5UJz%2Bi23nP3WSKyDdkWobgYeS14BkY1CLf2BfyvWgRyC2YKjDAmjv4VeVJvMqtcgn3yVGfKKbpA%2Fn1R3YYMlqRrBCjnsL3fz5%2Fhi1zGOhjWVnMUYyiveUOQ7HSPk2%2BgbPJ9%2FeKEK8CvR9FUtfWfU9iJeKqV7ycYBs7wg3wKmU2QFlAtgA2hhRHePP3pgtlclVdgy9E&X-Amz-Signature=ceedf04d296aba5d43e334640647fbdfef5261e9f87643a415f47fb3bcc87cba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

