---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZH7EXCCD%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T080047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECgaCXVzLXdlc3QtMiJHMEUCIQDE281A611EWtTkfJKPLKk4GYznwoSw9811mXdLckd9RQIgTP5G9pqUf1ujSNGun5kjAuLYIFRwdqKTAcMEwxcpYu0qiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBjvfAhfD%2B3udsJwnSrcA36HIO8g6gc6lmSkH7zcIgKMtHFR1w5TsfFsk80sYxn1hZ%2F4R4Qzns62%2F3ZmHZBVHiOSbZEmDX4xfYgxYBZOyo3AxRpf8TnVs4R3FdzqD982hVLG2Bkup2eHzq8zCidKM9yKuk2hHwZkf9OqG6Jn5%2BkKlz4QiFo8sCFQXpzT952%2FTImblH0E7M7wToaVJlhbsIvzIBClbhg9zW1BnvLSWt8CZ8hg%2BHSFeVCy2DTs40H41nRO5GSfgQ%2BAZfV6kwvq2ESij3fasscrXL1JWFsSeAFrlkcAmz4EN%2F4kmjfNxIbB2W%2FCu%2FEuisfQ%2FFNgcerVMBEGMeLN3StGehgi%2FJFsBQmbBqm4PuRDRqq5L9Qx2571LnNCRqkC7NYxlYz2Hb0Wg56tZJLE99TsSdrWYtqfOm05gV8pelOEP8QXkH7p%2FV5mYf1F6pxsbzG6NPRgsyF82HgkZkWgSPwGMJv06KrdQbVdH9sAL%2BYzKNQyfufbJ1e3liAeV%2BnKQvMrgSoDszDKCZ1f3uDCYZB7F8NStyzO48UqfiC%2BgBM4j3%2FqrbN6vS9%2BQZSJ%2FD%2FdjVw6jIVbqcCXy6ZbRVPsAlnargjjtANsDQtEiGSQLxU5ZcOoP%2Fa9R%2FvyJjdgi0KLXkcqxVX2MJTFqcYGOqUB0KXZUowW8LHfgKqKuqhwPh803MLOCZ0VP5qs8igb69jzBVv9%2ByiGrTKsva7NZJ0IiILm3r7C22bY%2B1INvuFFvFIdNBGBHwZp2yIyiB4rw%2BmsBqpMJVJHiTWuphpDrLOPKTtlyLL6szMcu97yjbn5w5Ww3nmT55SYrhXPKQ2Sk8nSKVaynWZSopisrP1AAhc%2B38cJRvYBKHzOElPGjXBS4K9QfeVS&X-Amz-Signature=5d301fe6d01632258abd8cae23ef53891fa94e04f484f458a0f8e88eb8b203df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

