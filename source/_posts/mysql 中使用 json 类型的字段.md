---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667UO7XC7V%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXlBVyM2gxTD6xtDnQbcre6u1eGuU5BwyaOBE7RilXrAiB5uUL%2BnNexNMMD25bsOvdfD9gexphrJ7lSJuAzzyOBhyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMrxIgqnRwF0GH3EeoKtwDyAvV0gM1GFYbwUN17dO66z7Kur96kGDZHVdeUVGn2cypmb2zjokhwJLRlMcnvM9PD2zaa%2FLd0ovMBu1EroUdH9Iqs4SL%2BqqqnUZvJ7Hzl%2BAkMujQFME1zIPk7yKfUAJcF9cKMdUrguUVspoKBBJbpf%2FKB46Om0%2Bc0jFaIFKRd%2BYyq0ozXMm3s5GEyEZaR2u2MeD0YUL5eU2pmQMg69qNpaxfx8tWDPYRBghGLJfzhWLSJKbD7t%2BXKNbkyz1igr7ILN6nuzoQlkrhp%2Fb3u4TGKYJUxYBrrZC6QnTyfhACEPaf9p%2BCMDRumw7zcJFaAiwvZo2mjx1fzjxrkaNSu5U1ocqcuIySIxyfdB3BjJ4WI47miLOANzrBIXFULP3J2v%2BsoxdecEhkHGIDA8RqekcHd7f0sYC9d%2Bsx%2FHmwNt%2FJVEhFJhbGmgQhUr9cmZ3diGuHeJ85Y%2FDcMqH8gCMY0w5XBzkYeB0%2BzzBGGrve%2BDEkprLJq1ATaxbgw5OCeVAIPmhQnF6fg0NlQjjVFOoyfIiEdGbdg4o%2BY3efkDCqgqhj7xFb7tI2MjjBq%2BdE0mrV%2BnZ%2F4o2Ofpz5%2FNOzPUWG6G4oic50tqzj3hpyMqIK6SVwd2nB5F%2FAFOzBTBsrBdYwxdfyxwY6pgHLemj5Zd4t6pqsCeqoMJiEEk7wfBdwEDcEzzUDWFHfySByXMss%2Frl%2Fl55NAJDT%2Bj6wPqek0HXcfXbR42ct03lmBiUYn%2FbBKVRgrbhWTJmB3KSaKgqajqfkWHSg5MfHdaO8S1u%2F69%2BbXkn0OZosz7rKIDv64DiYKVHNOJyPeZfjQqYKAFHx5lkMG2A2K9aGDRej4zYq1WcAeR9VaYHq1fLcXpKLZsR1&X-Amz-Signature=d1ba87619b937b968a3fb64f231db7206184725ab4db5389d3140eb0b1f6393b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

