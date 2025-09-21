---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7A3OMOQ%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIClwxHp6ZgmiznRIWZarCFY%2FmCMK8U0mgc5QwPrTEt0LAiEAgoHV4GxokWZOVqafks7Uw%2BRfkRhDihjZiWeO%2BWkohZcqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJVEZv9FEGH42j34uSrcA2ERTX2AZt1pLiRAmZHTWyBsBNOU3%2FOJaNRMRj8wMdqZ5jWccoH2Wx327yCsCdza8TjzY0H4yRG1cb517RwlxuqbcvqezDGE9bjg%2Bbp6983i2ilS7PLPb6LYceDWn%2BuR2q1tjpKCLsViE12foRASzOypsZMUkLCAaTOfbigZ4%2F52w5QwMcaCJPSX8SVBEWFRB4qVIX2xlYVWRxDbi4liq4dtCEWZ2xpjuQskALPi%2FUStLYPPVFUgcE5zcZEBgkgSoUhS0sDlxNTrcEyry8v5RhQpWWnO4f2BsdVRT%2BakRvqaOYp3Ep20hQs1lKLnBbO4OXJL3O2Qx3%2BcsZUMjJTr6mdvdwzHJeP1EN6LVeNcBpbiB3nzZu%2FRIamSK3Tl2S%2FO6JI5BewKO2dPvPJEgczk%2BePizpHAF87I3PxsRUkGG1FZju95UGAWqzFSKJyqZ4iLjIDq3D77RgzoM%2Fz%2BTnlQ08M3ZhnYbfQqcE5JKWucq395xf6L0NLsaFEo0SuCGv1kLNykFPBW%2Fep2n65okzJspaD7ID7M19%2FD3wnVvRnaPJeAxpTmjrxoiQ5y4HlPqQu68Vb%2B1kK5jxzOIR7Zh28hwRrzu6%2Bv24bY1UuPw%2F2Lhyo1VWadkUkPP39WCQSrMNHvvMYGOqUBlmq7cPTowfyP%2F3UjLFzTbAFo60I%2BrUbLSnqon3hoZVAb1CIK%2BDP8p%2FWSJVIrG0EOiiK%2Bkjtv4FuYiPsju2Zr2auaHwp0naT8stYILD45nJQ6ditChTXBP99Rg%2Blg4RYY2hoak2xLzYtH9iKqwQzzmlYkNzdMNxMggyModPyEIv%2BMlxKQ3UpI5%2BKZPzc6U%2Bxh0fpRmr0lviDXQpKftdd2tnLc%2Fghr&X-Amz-Signature=266be87c66cd08ec372489956b4e8ae8c1f3782e472a30017d35a275fcb3a005&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

