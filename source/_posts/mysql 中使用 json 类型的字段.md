---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RBA2HMEM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDRoHUgzXLWo8KGarvtmYoRvK3YDDLVjQEiYci%2FVdF%2BbgIhANlzljKLJmqaswTsAHTs0oMp5cKRCHEKll7kvzWWxt9yKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxWEDMAgFmfdmC6uuMq3ANYqW9xmg2ZhHfuHSBfarEJf7Lb5HNEnH%2F4syjDwvIrcgu7mCt8Fuq1PFHSxsvF1wJpeQMAQMRgqPcvFYOx4la02CR5sZVSz%2FYm3h7G%2BCQ%2FjT2JI7QpmE8nEdL%2BsXOQDFKJNgbIQas7ZGUQ2zNn%2F3uMKPqM%2BOY%2F8GoJwig9Q55rNVMRTkXrgajTffPESEENuLYHNyF4M%2FOBnRaDjpAEoDMEz5%2FRIFtvim08B8mfKPbYfdkU84CjmytowlIkf71%2BL8WY0i2S1Z4pIuCNYMsGNfdzf4nDQiDXTTTc%2FJikLXCLwwYM45vlja3%2B0%2BdU5BPTcyZb7OyhNKk2juml5%2FkxceUAkAgP8PmQkmx5R3YdRHjzzaG4A0fZw%2BcXLyWOmolHgapeJRdjrWqpGyRizZRBTg4q2ukBgFhh9WCRwMX7i0I3y7Tt77%2FIAy%2BZQ7ebHXDkY45WonehiMSGCS9wf%2B0LHRFlSMg%2FOHgG8pOr1uQYfpr4umGZ5pka1W6Yt3%2BvPCOqfCwkerbHOfruMlgvTePz8us%2FzNYTzdsjQjf09yOt1eB%2BlYocFmAszIMeULixTQdIHtkTQnJ6udKK0%2BezrzS9jDRyqpPIUY76M1m3JwZPvZ%2BARofAD7Ejdccnm35hwzCq1%2BXGBjqkAY%2BwHjt%2BLvu2IDC9xiiuolsHYz9G9hZJRiVLVhWgoikuWcbA1Oj6hlVlddfESPLwJbj0m9doPlr1KKmvsdCEInjgj3QPN2yIZJG9gXLoNb0JDQ%2F%2FsIX%2BnlxMGDzavFSHdIloY%2FgOTtm7WBL0BFcobMLlQWWWsgiVfQfw0jjqz%2FtNaHSoWP01wJ3quLK8XLt3Gh18ZT5YEfivA7sYZXrghF3FM6Up&X-Amz-Signature=5fd8d5c4d276fa71932dbedc685db5860643cdb981f45bfca9e77ab03bb3936e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

