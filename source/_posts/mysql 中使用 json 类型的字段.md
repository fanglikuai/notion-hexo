---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JHTIPWB%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T160055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJGMEQCIGNmbiQLzYW2PoidPiJppT6MS3s3ImAPD9OTyIxo102iAiAfV7KoHBQnfuCdoRcpFyfTzIQU78KK4BlljfL5OiW68Cr%2FAwgxEAAaDDYzNzQyMzE4MzgwNSIML%2B4Drda%2FDrUOb%2BYYKtwDmirQ%2FewNzCoq29VKUZV9IYty2K5FqG6IMr%2Fz7biksCEAw2mLGgbRZzVMyMGTZUeM1Q7dzrr78JuRV1WraZLwKf%2BKU6G8qjFLIURh0iutLgo2ErGJSmEtDryn2Tim6t5xtkGEO8jiiCNhu4i6ga%2FXSO71pZ8ltZUAQbrj0kmlE6majUiCJitx%2Bo5H13TKnnFSeiUev6MwpeuGs%2FrETtKilwPZbw81oUOHipGSnyEViyINc1i2FsEwyTufMIiWzWWnQbo%2FrLRq9K4ayfFLbY9PbzozV5tIgHpNcFWlp%2BOoBvYzZJhyg5pq4iIuR%2FnD1Hulfg1giq8Z3JV0nqDU7qX9bUFyZgTIL6lVH4AqlXAkRdJZqBEze3ISI%2FZr0HKIqi1J2zqbnGZ74TdHNHuN8HLiQJc5MRu4v3Lo%2BQeFZ2lIx2XRUNGm%2Bn2yqhN5%2FBnhFqreq6djcSkB9XdVzIixm5PXi1NOSFuJy5AWHk5JUrcHBc1M5%2BmmXjhxtKp0nvRhpc67uwRLKFeeNEF%2BBbvUvcPMUGDZOerKn1RHZ%2BFmQllJ8gZOncn0bAIt%2BGJYp5fjSnksFxJpZmiQvZBtAkfyugn%2FRNKg5c2P96jRZiGlQSH4aMdz3VDoPRMh3AKz%2FqYw8fTjxwY6pgHb6%2Fmq2MZbQ9U14Lvu1fXSb5KkbyBmScxjbD%2BuHyVTh8XsLrstMzurzbrWKOQYPanVy1z7Y%2BhALZcpAkSWLESuBQNA0VK%2FuHmi%2BOWt7nvdsXQx1Q0F0GUBS9URwArCHLhb7AM%2F7a%2BkT8KG5Jda%2BH3LZXKczLVymSHm552gi0iZfKwGqhGqedxb4XE%2B%2BVD587AYHnhPkevqUCPeF%2Fo3ahW0n%2BdL2gJq&X-Amz-Signature=367f348df2c92d4ba70e47e586cbf5c33e0433b5e4cde393f39cfbdab1f3b90f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

