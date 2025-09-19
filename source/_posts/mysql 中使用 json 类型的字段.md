---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W2V7VOLQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCQB4GQ72634WcbM7VURxyLuDhaujhS%2FpXTEgYb6BuG0AIgYgmVvizYgM3surNmv6018dQ6x6IGWI48AD9FRRig6eMqiAQI2v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBGwaWUwaMZANYkEdyrcAwJUnEMe485HiuIg8opDRlE2ZoNrfYz6pzU%2BsW9mp01R1qOE3xLY4WCdqugb0zig0wKQk1V5qt9Gv23qh6UwdIYcOUrDwHeKrLDIQnpRYNbabVPF4favNn92dXTy0pV5gb782ISxq6cNsC0d3Tus52qx8eXSlzivwgEWN%2BA2H2hi%2B%2Bl6AA%2B1R0Fgf6cORs3efHv34hQupe%2BR5%2B7UpPBrbwyDLD%2Bvtk12dpD48k9TAeQo8ENhsx4RWeamzfRnknV%2BoVHi65f8oivhXpgEn%2B3xt6SLd%2BJsWFRHae2goWuPZMiHvwRNvzoNuFz4IMpOC29eyTdznJO7oExiBmGxnnGzdGpJPeAtuPkFfhx7%2BqipZxKO%2BBS3m5%2FBHfNzIFa3fee%2FIq6o6Y%2BzZhYGTAOjDZzAJWJX5Wjgg9RzSsYIwLjWGd1pOMGtCOGvkLNi667px52CNQcePATWHAwd5XKvZ9RuI0qjhyx5aTGvj4Y%2F8CMX3OA9Gc2Soqfn5Pkv3ypBKt%2BJA1L4Wg0bMMaBODJflfuh%2FS9ayR9LmgZFtxd6m2LdYX5kNxwWUVNIs5D5oZ0ggRMBUvrPqAUG%2FygSszsClXvhU32Gz8kz4%2BBJh1y3bwLYdIHbWBXNIz2sD1OHqF7AMNaKtsYGOqUBoy2LVxhYGq5dWtF2klvyIFSuQ7F8b4Jgpu%2FYLfvWawuvKe0u%2Frxmit6tgq4XoGMoAPVYkG56BUQ0tAVRI%2Bu2k8vwBMMlYCnfvufniJHMvsi9rvOmjwZ7OSkHdYaP488LPBqnWaOH0OqsY5ckJMlxCDmWrLEqNSpBnyOS0cQLKqEi8AxkxRa552CdFzZly3nDV1qhhiDfw9jvx7K6xMGsMJqXUvmX&X-Amz-Signature=cfec28ba00f1baae0fb8387b8e901813d30b2affb71037091e58635fb69868fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

