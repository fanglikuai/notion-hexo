---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664K2ZFEN2%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T180039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIH7vDA%2BAfpg%2FOYsuI2IZmH7e6l%2BveKG0BE7Z6DbmtcTpAiEAgJ32s%2FICRf%2BgYVqFNSGdDPKZz%2Fyqm3N%2Bz1fij7Ld4ZcqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKFXneFuaYMYmwqmtyrcAyQab8Lo3ma%2FdKZj6BoMk7y5D9Iz9xk4GrlSV32SPPZqxEPdqTPZhxJm847pjSkbO%2Br5fd9puL8My67uouE9WztWlFl1YIpqk%2BEvv7vsTNLrt%2F%2BGtv8AJUWT%2BGUVyVq%2FRiPh%2Ba9Kkw8qd7vVQ%2B3mePE42so0daVJYl%2B1KXne2tsKJkLSpekItJATQOUMHT4wqgJovfAu2UC1TdsWzCOUu7MMn%2Bf8OthvVimc6KjxcK0gM6o0b53GreiKbfcHaBzGaF1eOdLVxQrLQ9hsST%2B16cBPQB54rN3Rl1Jhvast0fpa0c%2F0wxmGs2HtUWHWZaLnSd%2B0zQzKRSPGEXdHqdxduj%2BQz8FSBYLPlqdk4Eq9oiChS%2F6WG4Dt%2BCQWmPF0obCKgJE5S1ZfWh7fNar1qjdortxh%2FVbmPKmlPQWOBifRbm18QbWtps0qGXFlODL3ObWEA0a3MTE7q2kqro3OwD%2FRSbfWXoyx%2Buc%2BSI2MM%2FHqJmXHwzod%2FFYvh244oLd25%2FUEJQnTuqhE37vrYTAG1zH9DoDb1rDudfW2mPTRIekFhxLBNhZ%2FMsKiSOJGTzsqKr6PcJD2ZNhxVsebrcUjP0q1zJ8HbKyBH3RmXcVGqwaUMzkC6%2BllDO7Xmgy0hlUpMIjH8sgGOqUBE2CC1c3efXsdZvoJdvE33rrDQW3ycXX2U5pt4oWHKEFEo8EjQqm%2Fv1T1JnyGu3I02cdWqek0LzZOUBZTr1Sge%2B0jT8e0jAPsuEBSzTRP4mCnPCj9oR1xaRC7UdOusPu%2BcX0etEW64tMo5PHI1ZQ1L%2BWfxygFhDsysgTkWCm%2FyzM4IKi26Y78GlUVxmBTHKsGLoR80cxq%2Bd5omp13YBUKS8Bw0YpW&X-Amz-Signature=babb9a811ab14923d53c875a4aa6c8c9bc1d93f7108edfb320d7c182aaf8369d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

