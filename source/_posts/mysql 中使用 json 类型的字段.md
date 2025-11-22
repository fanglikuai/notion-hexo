---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFQZEKPM%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIGjm8Wa7I5ek%2BmxoCK1cmxF1HHH%2FRRTFihnRqf4eNkWwAiEApm5eNzUCLqn5YLDQcuGuwCLrwPqZ5OHGSyB6z7OzqfQq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDEv0pPasOzjH1w4qHSrcA6rRfjTKRheifnBSdgUW2XNRAT8uKBVpy%2Bz2GQVCx66%2FBfilSFzFBd0gXiFL3ERlS03PzWelq0mCtiTMXDikk4PKMLch1vjeR%2F%2FrGbBUgqPfGCXsbWf2wiE0oHG9OmWx%2BwmCDnSfnAudjOJL%2BtItLBn3Qrin1piVvXQnQK7bHck0UyzAC5wbduqutgrieUGUu6Quve5CRJHHwsPPbFbTSUsEsYsF2u%2BeCp%2FJ0BHpjuL%2Bz0ZTo%2B1EfJc9hlI%2FNmTc%2FyCZq6qE7GZ8Sl9%2BJjEoPj1XLYfu3eLrs1Vtu9typjNJTsyWPc6J4i5Yyh4pAmZZKb8A3%2BJ9mFgUCKSw0BBurAg%2Fgp6jOQFFp3GA1wnnv%2FNPVdJibS%2FWxg4NOr1sGz7KG%2BIv9Ezeg7b%2FmGlDxAV5ucvCbT6TH0J7yXD2yW%2Fb7M8%2BGSKVcYzfCQ9Ro1obAjMIClopUWpw21qamp%2FYbSiF1r5TCOCBVcpptjcJjkJapu4w7BVHsa9MyNOMyxFy1UZxGOXgjNslJqTeN1CX6TU42Sk09PEzYiGrqVTanECxNEobtLiTSFCHMMsSf0%2Bct9V7jS6pilndv0q1Vhkq2ppWY%2FcHRdYpOG79u%2FgACzvYpS23S0WLtQF9IHfHzcIzMN%2Ftg8kGOqUB9gDxE%2BTkB3m1XJGQTIMd%2FBkvmj%2FTYK2JPC7tUCxbS8ZR1%2BXiJqahKwFxffB4uYUs3cz2y1yELVzkr4lg5Jc4JHBZHian7%2BMGY6RixdHJO51B4xSWpAu2ZO5o%2BopTthPCEugc3JbGY4bhOHaLTcAp%2B1X6Wtvm1IvPzZL2%2FyMGpm6zQdnlL7Cz5cxYlvO2%2Fp685S%2FlDC13HglDcQddONDj%2FMJsc53f&X-Amz-Signature=328ce8eba81f47222aedfeeef86ce77b2d1a84c5f40b3550f497caffff111fa1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

