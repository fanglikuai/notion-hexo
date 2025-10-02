---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBN7TV5G%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAi89HZTGGbnW8NQ3AkjQmqu9mLM969VXtBtQNVOIjLdAiEArkGu6ov6WBSkGwp89lz57IfkVUlAPcavvmz2u2D3iW4q%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDL69kUpvf6yima1bnSrcA%2BjEY9tqlE3JKlaWZhWBeG3%2FO7zSOUJnYYLKhboh3Q2rwMf%2FQN2Hols1Zt2ZGLhTEmXFNoRW84YntUIOsbmYLFC2feD84c%2FgqYDko2zz3iV6db%2FzMNFA1Fq%2BJMJkgM0naTsRDUjMP4VKG0UryBbldxlBf7Ba4uRcgYFMxlWkFUnrPfqo7OwMYT4IZ4raEBWVza%2Fm5u1ZxMIVbpoR%2B80TkHrg1K5NRP2YgMcLZg4lprCrQDPWvfW79LIjaASV66YaQBRHlGtMnp7uUyAJ6q1K5F%2B3Rj7yrY56AaFfsnBGRWbJiswZIk3AKdRWjic5lze7rxnjQNeVkqbbadTlA%2FtffwZn5G68%2BPWKEzKUgZ8wbGW2FeR%2BE3Mthfx2MbsbRFb%2B93QCT2EyrPCkpsoaHjPdIsiMEjkvIX%2Fw5UJesW4Bxb1PBLCweQ2caep7Ql2HdBGlVPl450Kx9CROCvqbq9K2zQgrMZDsjDOZ%2BINNPdejywLoU4Rea7rs1j5EbxrcwDL9DRwlKcS2ZkXTW7CGXxeBNAPp1uQwsUc9KRD02i%2FWurdRG9K6knuluNR9%2B4Jk4OD8SP6g0LBbX4kSxA1q1%2BkWJLGzUV3%2F4Z%2BIL9HKwYcQGKnN9OERCupWOs1nJW27MPva%2B8YGOqUBD7KaDKZXcp%2ByKhqTMLlL%2BRAKRTohcIcvtSYJG172g3agclBHUvo8ZrhU8CNRzkJ7y1pd%2FAH0%2FMUVzdRoIjebkrk3coeir6AjScrRGCr8AyTyaDF2w8seT%2BjGCi%2FAqW8p3hFK6JaC0dukSPXTmxGspSu9OT6mqvlN%2Bmnk8A9zVxk2%2F7v%2FZ5RfLC5NzfnXkJz6ZoibQjv5tMmUBCyWTBSE7Idy8nxT&X-Amz-Signature=c5246e8842db66ac3eb412670b532c0601c91289b787a070218e1e82efc03e76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

