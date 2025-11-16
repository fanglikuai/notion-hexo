---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WXVBRIP6%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAeEVsrkhtWlGT3hr%2B4%2Bk%2F5gMir4Hc7gtqLBUiS7xNRjAiAYJQ9MzQE3uGQDDljcz%2BUDi1pY8ppjuQeJzOiD5jod%2BiqIBAiF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp962XUSGUoNzTb76KtwDQxl24mFxw%2BTyNjbv4sXoc8aVjqupXzD2CVIXSbDssbjjk6W3Gt9Y1InTwkwfdWPkKfNmAA4xM0aF8WSzQF4zUow90nlaSNUT54EUvz5I1uHVPpitAS4Hgz6i2FdOq62E%2Fo5rLZDBUAwJzzQ3MXqyYwL%2FFpmznXuropAnoN5X2pzwoUdZMoIC3h4UOl9Utbd%2Ffb4Tv3YYYMmyjHW0FbPR28tcCqMlwwZvWJ9JyTfyHed8vELiT2tPqv8V9gSJHBTLyOz9VZDJi%2F%2BTNIDEDCPsVPOgx2ItpLm%2BTBctB3JrzsQdbMJNBvAub5zMOzlY4v7zfw9pdUjXiSXRZvCI2bz0QIsqk6ULL1UBNRom33r%2Fx1DJAO20UXBjagBQsAhH8%2B%2F3slu4leVjCTO%2FvKqdMKXqVs6M4%2FHFgcWrFuTXdfs3zu5%2BtuCNS8id%2BdtGtLjM%2FUOAs9yRRYk1T4qHSxCKysFzGTubxGcxUlf34Pby5jbwYQ%2B2r%2B%2Ba6IYITMSZ8%2B3%2BeSfJEZF4dVwmrtxgWqUuMrapKLnQa5p2DoA8hgORChfAIMGONNhRdfFz8Q1%2FAaPtnTowIqHXpK273WCufue7qNmNogJZD0t1Wak68ZoOr9LeaJmSOu1cn7WgWpIEY5Ywv7zjyAY6pgG%2F2nmd1qJyskwX2TBAGgO6%2BIbEMkeSXXEjX0Z9dVmTdEtWWzgCFXfha7hBEDA0qZMatIQ7LE%2Fv1nFbUfcQ55hKgSSbYGDrkuNCgsNjbOeQPGkomhzVbwxKET7y6hoCfgzaTAZ4h3chIFhW9BQlIQe6UZL6j7lcnmUksA%2B2pAv0xGFkVVT6ylT5%2BTcl3NBFOJ3mfgO83l8l2KatVC8p%2BRRmPP36K0D5&X-Amz-Signature=23b12bc378228efb54908e255abfd311c705bd0b1ad6f0b12ed0322d9dd52136&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

