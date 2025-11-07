---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2TTP6SW%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGzbrSc1OaMhzvBhoBa9Gyc7wvRphVJjmXASEXufoZ6aAiEAlnSWv4xcoLedGxnODAJfCRa27pbwMLijtl1SsqQErhgqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNFDO56cRaaMpV92CrcA2Xw%2FTrU0mUhkE7du58KmuAs%2BqjWZY9zbtbXn%2B53eiwqj%2Fa0%2F2TuveAFNqN3vboIa0cgHLxSj5C2rWRkwTZVvNg36X5rrD3TU6%2BmTnRyjZWYNVUuII3iyViGYhbUUjmFABfae1SZ2D%2BpXTabs40dZI72daFVQmTEDMVrX791v4AxwX%2BzKl8E63EAAOh8hjp2gL71fsX4iX82bGXozw6A9MGdIfv6zpGaaiKeIZJBuqJit9JFEEDjhLBJ0skaaNAlvx2b%2FQzyPFwN7Oje2HH6SR8frb60zhMDDekGPbiOnB%2BMuGMPw25lAfLCiXIALV5Zx4Vdr1xDX9h%2B41IeelaQZqLjAe1C5sRhavzWHKPYaT6jw5UuDZS2v60qVM1D1L4t%2BiNAxskDe3ud6ZUwKydrqSP7YW6eRKpdVWquhH%2BGMzdqyGHOFbDE6fCwTBtRnIGzRlkpd8dkV3BHFRp3flhn86V0V8LtsRI4GZs2fNvyrtOtWPPuvJEn76dk4n7F8klnhser6Z8pPswtXeq1pFI4sERpxvTvqfAF%2Bw4HbHVHH9J0dBl%2BztE9W3zPZKVcWoxQ6KXM684v9gXiY2dgr1Sy8yX7eMeal2EUmelgGwmr13APZprGRpyVAszWBiQfMITRt8gGOqUBFZzq6HD66Fx%2BaQqYOTyazLTu1F%2Fokb0ACfueWrooPFWelUZ9FqQlnJAimXeA547VWADAIhjXXVfAkFXsx7aoEUVYLDkji%2FcbdNKyzw%2B%2F1CtJtYDzU7idXSOrtsKujj3fpA29rCzh7r3tfPIzRxK3WbC9vg0cPD2TuOTkPHVX4bBxX9jQ%2FAwELdWBSqnyEuYSKF4jU%2Bf8paMYB9AeTR%2BkBGn3s4Vb&X-Amz-Signature=2456e8dc616c6d89f0c005055d45570672922efac8db72061268ae6f08863f53&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

