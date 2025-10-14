---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3ZKL7US%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICu9yVcnD7kQMwAWrXoZDPd6aFZykNzjCyy4MtvEv70PAiA%2B7PulOPM2tSj7sEG7blkpMS5EIoRSEE42FaSKVSzsDyr%2FAwhTEAAaDDYzNzQyMzE4MzgwNSIMD0xwF%2BxxW1b2veqAKtwDu6T8DcmArpVhrIbHkHPR%2BzxG0Jg%2FM8em%2BcnMsZLmTk8M6WUDduGvEkFmg1yBe6azmwzI1GSPMpZXskii5USUL2B2SdRwFiNjrQ9PqyYup5OFgBpJScXqU4Wp2xRcVk8vVOvsaP5g2eW2MN3vmTwtnq%2B%2FhdISOK3GJTfTwjkRQzHpLempy9zXa34jxgYlcSARCdgl4sWdlJjRi7jl9ua2BnBk41nZKyXyk4KOKMmZrMB0CSahY6DUgijkEoBL1Hm9fYkiLktY5Bp2LcyrcWqTlMpyBQW5hzaqFcdu5JpJ4qmhUK8VzSb8l5jNZPyQzKHQlPqYA4vJK40mS1iAEeX8Cc1XO9Rtw%2BwkSNuf1aS7DvtuUrh8NilECgOn1CO68gz4dVUhcfxfDNONVLHBG9ZUXKQTV27zLqos2SZL%2FQvnZrjgfQu9%2FYeXJ5H5nkSuuwZv%2F7n8Egd9x1ArBNkjnNV1u3%2F5rTmb260z427TOwjWYvx%2F%2BW%2BL65rzhMdlVd5W4482ttzpIdqDNBgitQolQhzbpT8RZdA2LXzVQs7GDsmTzclcPnq7xE%2F%2BtQDgPYs%2BQwsBU5YWMOT9T9a9FU09cldtBfq0EFCc4P%2BtxcCamWTsYNScWfjs9DKGC0iIa1Ewm962xwY6pgEGJGAJf5U4dxGaaducOJICH1gNsuU6tM%2BE61B542q5Yl8ocw8%2FTQjw3D5QX73fBguFPZfkjY4DJGM2HMyGQrZ8%2B%2BNi3iafYT6fo6hHntAifjEE8AXvpoJ7dyhNd9FyeYzbj%2FBj3MPFXYhHHvCxTYDIYiBIAbD0mT%2ByEs9R1qxS5Jg5CQ5EFJ7wbrAdeoPqXybO3vKWhwIdTOyNG51%2FepkJrYRztPwQ&X-Amz-Signature=55871a06af685f5e31ea49582bc12f2257e764d61a30167b0e4178576b75ea75&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

