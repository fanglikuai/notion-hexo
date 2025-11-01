---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN6YZVUA%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T080043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJGMEQCIGSv2p9ISY9DT2%2BnWdNX68O0Q4LvhTz63xECUWvF8GbxAiBSEky4TE1PuYwc%2FmU8SaZw1UZL2kQnIBBxu4Ft7ttvASr%2FAwgoEAAaDDYzNzQyMzE4MzgwNSIM8VEQZeScQ6SIisXfKtwDCaSqLd4XppS3K51IOj9%2F2lA6cECafQ3hk2%2Fr%2FcMy74dLElLnXhT82uGZ128ivzHnRZ5B97%2BdsWk0yN9PdlrHblIAMPwvbriMdQ4zXCS4TKu5jtuo9WHNhymnypPTbSk0IbTjPXl3qb1B%2BYki5rHBs639pqFybks85J8IWw%2FDKihZd2%2F89CWk2bzaa5mYXiajhZ%2B61FCTg23LukZrJPBYGsJ3jvUQXES4Wtbet7uJSSw0Dh9yjj7FmvferZR7NDr82zAoparzrUKJW5JPu2RmHBA1vlD6YS9QKThW%2FsiqYPuQ63P15r4djMcZwt1LhCfkA%2FCgQY4uuPhW5U79M%2F08C34UOonI%2F8xl6UY5kApWwYpFLJpAG7DaCxmuplbY0YmbI9t14IWrIeB1CKVMhaRn7lUxhWSjOItNxEcJyErUCT9GmD8OInlDIE5Ci1q5rBun7uKW59NsfagvBzbBL0BZWs32V5xQ5FeMRk5Ik%2BsRsjKfOZ3BwsXDggNhQFRGQEprwcM38PsaXGffJFd%2B84V4rNXCT7jq1bRZIGqKDGOOfX5%2Fr%2FGXwZPpU9PDlOtzo3%2Fjo%2BYVRZGx%2FaSI97Lv%2Bom%2BUC9cUlJqJ7lHK6GUtHp3feweFRczMJfFU1lnVE4w88%2BWyAY6pgHiMy6msfECk4v1VZby2JvzY56JtdXCn4H6aizhwc62sE7CwK9Z7lRYVwYpt%2Fpl96VNGTYuS1H2pYCw2xfywE2P0wMhnSnkEJgv4iv7lbSqqgzy1LiiR7Z69jvNaJeSYcHKAdLsaAVO8dAlpXNPDFGwiTSI4YtSOCNq4pCZj09jICWWbVeBn98iuC%2Bsgww6AhP9oSswWICcoMfv%2Ffu7Es6ZIaT%2FhGRW&X-Amz-Signature=47279df32954725cd3f3b837b2b05520e85ad0e8731b9258eb47ba157b4452dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

