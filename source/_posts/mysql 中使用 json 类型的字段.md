---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OF3KFTI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDOvfYfuVKNq8kG%2BPNKM87UG5mZQRoQBNDuk%2FZf7M1qPQIhAKFFak%2B5mzt0FTgKve%2F%2BpfOXdCaLYMvAj3ICNAvt7SOTKv8DCDcQABoMNjM3NDIzMTgzODA1IgzXzIRElZWZK%2FSBGhQq3AP5Itz3OwzBAXLItz5QbeY9HOb717%2FXs4%2BHgtpqEOsloW0yw6sRm7wyU4AIvId6Bg1PZuqCNBiH6jQKQG93wFFSCod1a3yvr9xuriu5Ksa2ym0792FNKFY%2FZgvtPoT%2BCeZjs47DIsv3ekCocmOI%2F5eB%2FlnuRWszYlDmgr27kUy33z5jWu3oHiITfPJHFh1ooLEr3NJalEKpUj7c0quDq3Gfxg1u5LFeL%2Ffu2mOG6amYdouP9n22QH3UXB1Br1u5zUnZEAIa4Z6%2F%2BzxO%2FsB1I1Zd9igRK%2FRMgyd2peZDP379%2BcwLHkbw05PN3kLyZLiDwe%2BQ%2FbP6ZmAOA8%2BVfChR9PdpvabwKrED7w1EYNY%2FRCoE2JIDgfHWj9rWO6sje6nIeULH%2FT0Y2STMtKa1HnvmqG2ozMiSWKWDo%2BNYUMphm3KQnoiWBq9OCDyVLM8JJw6crK3JVvSCfncCRhCeTA5AePCDRAQkQJhFq4VoIGra7GQvfoeGvho1zUEUTLSN86MdAzZPkdi4DGnsGAVIQ1vpBMzTHNu8lxICoSUz5okyWk3qvUkGTRhOnKjPCQaqThURAwG3YfLm%2FM9MJhRTrU%2BBh5lPiPKVdOVQB6OKwParHL7RFBnadsMT0DThm5ylrjDT2vvGBjqkAQXu85oU%2BtyYILAqfU7Sd5Ai4pC3%2BEa1IkwKGVqr5tNGqH43Mp8guSKQWHgVZBVd7f4XfC5wn7R6zSkyYKVsY9gNeOSlmWQN7GR4ldDFD2FFlC79FHuIT%2Bclmq2xE9OnBVT%2FCJBTPramRw5eMEgopQSqvUiuGoDvl9Td%2BjMMt3ipkETYYeNkMUWGxaUTb9Ku7BqMYza15dhmNbS1Cyu%2BYYXyq3Xw&X-Amz-Signature=b44b086d4cedf52f73b17864a94e0d28e869ef35ca9cb297c8c0151c060e482a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

