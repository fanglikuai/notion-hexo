---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AMZ5ZJE%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAnKsel3d1bdtDCMQ3mYe3U5tusTPLyRLCPzgvmRlMwAIgHMxP7Trr59hfbFErfESfXc1sJ2t2AqxHo7ybOe%2BrSwwqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDB6aiTvqPYNMczj44yrcA6nHYTdKd3Z4eGrKnzoPdV%2F%2Fp51ja1M0m9rAeTLxlk69Bw9KbLkxu01uSAovSL2AseLFkucvzqZbajkxM7MdkL568GoZvYS7e4WtNMtVi44XcUlumAgvxdkdVn%2BoPaz%2BG%2FTlgP7Gfa%2FMYCVCV1GP6P2DRdXba7zPVTN3nc6uAv7geFXh56JdzPhVCkRNrRVOBbl3hguRfBAtIL7DI0o4kBgVukkW8aYw4jQ1SwjiUtUZEpaV4zyvBq3vGLG9bvCDeilD3becZF%2FuRbkeUEdTKbaq8%2F3kJO0vXdznTj%2FunqE%2FVJTFJgc4KtsuUmhbXD3D94fsT7au72CkvwWhbfBJcD416%2FDlPILCbW9YM2GIaGG%2Bxysfn9siPn71kg24ulBkFTwn6hNQmw4zSj9YwZhemEIWjx%2BkWQsn4FFyydxfiqpTvuUV5%2FE0T6T%2F7v2cA1XwjzIBfTTSz00AvIBJMv2m2YuhN0rbhUz6dF9FHlhD4dZZIs4MMozE%2BesC4QygNrQReb0nC2MGA%2BGjlQpeeb9b8dMrSP4NqXVHbg5Ip%2BjJK8Bg3CzUahJasbP2ct8izDiz%2BaAJnym6hWn14BBuwu9HapH6YY8d3Mb7q6VpffG8ThyDXcQqy0uogERI7m1nMLD3%2BscGOqUB0hEmwbMxaqvQRaWBJCVMFf6LwUwVY6KWAaml1OqAbOuheKEM06Am%2Bfk2voN1nPEVJ6FzJyTOiSn0em28koHdyUWzPxXB9sj%2BQ0%2F%2FFVjwDkRdAgvUgxvi1qr7NHzkkGJzhOxki%2F5EMdTq8O%2FICg%2FJhyoIYRxw0%2FLQuEtnZuPiLL0KMScMhKnJFaAsAlaoOUY6LD5%2FqoMGsRrsIAq6irLRVhPI4z3l&X-Amz-Signature=51cdac925969f88bdb5503197751d269e1f7fcaed1a4a1e11bc1d7575c61b63d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

