---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LYFUVXU%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDAMvfnkRrk7AZw%2BMU5yBI44rO%2Fvzj24eYHj4xQql9g3AiBfer15G5Fv5R8NGiubr0fdxYaSOV2oH9EtHtMd2R%2F8LCr%2FAwh%2FEAAaDDYzNzQyMzE4MzgwNSIMV6EVZ%2FQGnEguXt9%2BKtwD5H4jrL9nUaaeUfe8gzeLHqDxfu7Katqu042Bh%2BQo6mHYVImmI31w1KZQjke0lPvAyPFNZciDgvLfg3jV1k4gUtUEtTdMWzalwaehLwLfj%2BJqgD%2Fny5SniBDjRkdwxBuyrNVTpiyPuovgoE36%2FTH6KyVbMZOhbMhYosl2bLziUjBnYZR1P%2BuS6t1KWB%2FBqmWS%2FM1hO%2BB2tSU1Lb4HnO5Ol5sPqHrlqJ4byzJnzR%2F0hotVuDJWCCPW4YU%2FJggguVYbc%2B0O%2FOV6LRVZYzfVxDqD7LutWtxKMP4VQ0qXDMRJkRw1Xad6r08ot5DkH4smrla%2BF86S7B42a%2FwvzK7eWm52vSuVbdftKU0GRPJhvQvsC0bEUdMUTRVJBp7s1%2F%2BAnOlLANYZ9Nos14JhJ7H2%2FhCYJ8ZnEIzkqXJd%2BqZzNm%2B7HUfJ5IkYz%2Ba5%2FI8K5gF4enPuS9r%2FgQ8eWwit2zmSt%2FZAP51vbqEf7NsHTlLTwWTZnvZj%2Bak6HF%2BY%2Bn7pVPoF98xq9M8lNHLTwt9Ygq7ftRyqT%2FHQTbMwy6kEImHpPWQwt5GFL0wMMDGZAeyiEsz0uYwio1kgz434Ig1gHm%2F6uwJWlJZ2jDfVFQEBsF22l9xYUmC9pE2r39oRdfWKGQow6uapyAY6pgFcDmY4OuIoDkxKKlUce9mFSVwjgcxdB%2BFozPtHhuuOPssYPv9vV9IdQuZzJNx11KCqjCs2P30POEawcolQe3o5nQEJbcolvhIaBxTt8oFmk3P0gk4Umi51HD41SRHc3uiFwfJ3hnm3JWdktTk2%2FyweoTmoHTvU%2F8CmMWP2eL4oKBtDWNAq68liD%2Fn2VGgDQHNgGKN0CrbFuRvOynldRg0yWfsdkxLH&X-Amz-Signature=721def7be84a9dd3760c4c0dd70475c17056b05323e4cbaf2025d304d8ab0499&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

