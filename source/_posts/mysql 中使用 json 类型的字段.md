---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W32RHWJG%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQDTATEOwraqGAmonyszGQsWK7OajEqZo3S43GPn65c6XQIhAPsGwkf8Pzlw0uWnTmzBav41cIf7YAu7fELsS6eGXZ59KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzoGpR6xWunrTi06rgq3AOYgp7WgXVwMa1qxZ%2BdrtVCxrGLiVswzTHFxi1XsPDT5FSp74pV0HSIw%2F6GJv4eh%2B9eYIJgqfCdBsIOLXo8Krd5SvRkhO9IaxTgot%2F%2FZUywhgycQiHSJoqHMxqgMG3bchbTe8FJBJX3QcIMPSVTfsEvBjeMj6rwPoO5bt3dWJL47laPveztY%2FnZAEyCA%2B6ck1El%2BZRd%2B8JBwhuwF4h3km4HYkaj3R%2BSA8uqi9NCk5EFBCv%2FigCHh2GTEAyYfDZab%2FbVVwyUZvYzp9VwRiTuYqucs%2Fu2k7w33mRtNBfxKMNKNd4Y%2B01MAUSll7CB9RzfHcrDnG3JIx8zjbaFFbEEiJONNA1KXyhpFsZJei2D4g8S%2BKEOvf7zRDoQYNvl0eo0VknLt2sGLHJu%2B9ccfZ81ZWH1LqkmIyCntXh92okV%2Bxe5YiVghbmKAJF1qwg2LAoHTd0mA3bH1Tg9x1fneB1uN0NdtYSERrdz%2F2b7sQGR%2B4iAHLWvJmiPnteJFP31lcjrEfHd5w8tSVqi02nj3pS0PnTCqwbnMy8Zk3J6Ox2t6jgw7JvoIkFD8VRYs8gXZnd4Ea60zc%2F8PTU5VsnDTWHGCojKbG8JUBj5Pk%2BOemb0apJoj03GrQNdcl49Y2UCejDN%2BNXHBjqkAX1lXLGGPr1rVfhgjuu6ACAPKFuaFsDL6YXrekGNT5a9Lzo1P4HvMSDz4fWlw3KK4EaQ6wjLl3astfVXlnqrZcD%2Ftmh%2B1I3KHR9k0tTohE8eLmY7nRcjl4XEKWBiG9iatoY44Ov0wSIHCbNYVpvkdb33f9E8waSW2nqIIoYj97kxRYLqbalMoYn2D355WIPVvcVNCOHMo7wFLmq2W3SzNntFOOFH&X-Amz-Signature=69629b96718004e2bd9a9ce9629ef41bd8c8c2060a908d2bc910dbdc6e2103c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

