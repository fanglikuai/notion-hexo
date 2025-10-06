---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7ZHWWDL%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDq6hZFDkDEH4PhCqk4TEOBHMMfZgJ9PYBlCLkBNaRoogIhAPnzkpQOVg6dDIL4ADQxjqzVUziBju0mqtO7pPOT6AkxKogECJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyVSblxF9Cg5gA4aLAq3AMLWTVPcZfnKjVnXz6jxYKTVwabIShOWkIxfdTX8TxNjh42w3JhA%2BK3x5XBxeNbhfTS1%2BLozbKvk7d3mVWNs864HeRjwscNqVXkC%2FdWu5GBuAfPf2yly5f8vXQkWWZnWrz5kbr4%2BcIhePFQoRYV59QofzfxEZ5aLm2d0iu6URAIBJwzfB5hqbO7B59dMIgfZnTzT95kT9NaQftLVFg5CcxlKNeXyfX672EQIoVcWdBgo6FVw82JU5AiIHcNAf%2FLCNxhm3aU%2BAipJYFetxMviW3q8YEi04iJGZ7m0%2FG4Vu4BDCU8B0NrYJ0HE%2FaTw2wuO4mdhRa6kaR56JQLSZZf6daGS4aJAtFqXASSYi8jOwjGNgabYH%2FUmaI3EnB%2BIQKMl8lP%2BzH5stX3pT4JUEevyFG0Ek%2FvkgdX9i9lioHh7S8%2BATgRDKrHHfLXCtFjR1BEgh8J1%2F%2FPMZCCSKlu8vkg2HIemQ88SaeM%2BFR2BmA%2BtuiTPndBYcWeI545EFUgrUQYL5m%2BK0DGH5niubyC7vKrMIih8nexlYhyHEpea%2FysNE0Z2ENEWLNanIq56vlNwJHdb7tM8D%2F0hJS4lpO0BEawMNxaVYGDSg4IVhngB5QVs3B2d0BjUv%2BT0CNosTSxDzC5tY%2FHBjqkATB1ur2DxHtYFaUhSWTjJD4gjDkjVXRiVNyZC%2B9i4Owniqb8F0dklE%2BvDjoxgUD9HjABqoI1QkpOewH%2FDlwG4jTSIPfHwZ3hVIMQ458CM43tc4NHYiWiZYO2rYzvdd57PQy4OgjDn39IRAAIE24Kmq8iqqHXegePQYgamKDIxbvYKr7Ih3cg2pTkWZNiu%2FYW3mSbCSJ4dus%2FsWoFBJgg1w6VBQUr&X-Amz-Signature=9f39d633a524057a5997336634580b757ba4f79a14f1086648943b6975f48b11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

