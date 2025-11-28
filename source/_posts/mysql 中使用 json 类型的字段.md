---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNQBNIDN%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7em4BMG6rht8gYjHzvh%2BzwVNP6UFqF0hFhNYZEXiv5AiA10fw4cOGaPMIo%2FTKxMmaBIySbDDPxR5yyYYdcMZB5XCqIBAi2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMHMNTdUY7TOQWTOAYKtwDZIabvmj%2FYOQ3H1M20GhZCWfKDW8Wp4kmm6Toea8tY3SD9Y0bw6%2BPfR0CC0HLXzsQS38Rw%2BYdlX1slX2zqfRrDKRMOUmSuwJ%2B8yq8LTdcJN48QhdcSy82muBbGLLO9T98tjtYJBDL6eHIgdvO5BX4Qp%2FtfXr%2FXt1ZMAMxcB7UtA2iXWWs00d3bXVZ8hTz80IDCkd14lINg1l3OEqQMESTzjIEvXXoO0eNE%2Bc1ZnJWEYyJgXiNTU70Sjeqig3sjZvstB2pvu%2FucJwbyWfO0iINeumna1z2ue0C%2B2IUmDoFKMCdGpjtdXqqlJwlUrhokHigMZUV%2FjZYyiwY%2BPi8TO4uaa1E3sQgulRTpp7w3sMi336cqIH4boc1vL1bROf7GIYEWtThoub3u1grdl9vP4Ygh1VG0eMGgkSbjn08ZtpSgRB%2Bi61NZLRiJVVm9C43vNG5YuusO1wz2Ycf5bf9RJpSzV9CRHu42nMolvQ6SQJJ6PUoMNAFCzZgsqe7vNb1JOc6WepO4Wji4T16FsJgr7c2sxA1EVcbwjUNU6VE5gjDoB%2BuUIJ6Dgp7Z4kYIum6b0HTGyO7pgIBUWHqeat1VkYQ%2F7%2FJUMnImjzczjXYvWYvtrjJXccAJ8oUdOteUSsw%2FL%2BmyQY6pgFF4eeTeWKjcoN8mUoPHiytV3%2BE3e7%2FVxAkHXvtPV4lxVsDmXWa8FkgDYvHkgn92FdJBZ61ViHFwvaMZSMxjm4ODKjz80N0CsBWdgCpyFMPeCgFAp%2Fk6hzyaXTab1FOrXxeebi3gb0OkzIXpv%2Bx6wX0ewHW4KE%2F1gi319gczZ7TMnyBPVcFcCslE9G%2FTz4Y6b9Lm02k6tmpDog8xEOsPe7hc1Z5a50D&X-Amz-Signature=2815c5cdf3036b5a724a11a8c83bf63b4f2f63da4e2447786aab8f38d9f7f898&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

