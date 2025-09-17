---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZOOCMZRG%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIB%2B%2BIgV24kYAWUGqnOKKlogyHRQm1q9sfTzjcImI5%2FZ3AiEA%2B6UKZmjtl96lIAvEFTAbrI5VCtmj532LJ1ebMK1X4qcqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHpRzLGU5fJrxUybnircA1Mhz%2F7plEiEpjP35YiWG9NkpI%2Ba8HVBvw8MzsfhqZzf18hDB1f7ak1eaeaFGpdDlQjuHSdmxw6RBCYboxtE4c2yaJ%2BAyRQ0diTbe1D9gT1JplQ3VFeIZFxGs0NkLckMetdwhC1hArljkGWyi4mogWezmy%2FGFdV3nLoeXou3fj%2BKGMpDdZBCh20sJOY3zYlVTjqqUcfyerh1l2IkBVDt%2F%2FMUc96rP1inXpMjb1%2BPwnef7tq4nnvIvMFgC9%2Br8RJYoQW9pw%2FSbCy%2F8jgihdTlRq99nqDJ4JwcBRpIxulNiKvgZ%2FvGUHOVQTDvN3pf51m4k2StOFFk7J25bDRoIdZqgsFVjyDagw7gx4CQDxpQ5TOjmwVTCXmY8FEZS6kh8hGPWj0JOnoWDlmidQ3IOrLmLH3noeCYSI%2Ff5gWHKSQkGUMnrrVNQtbBfjn8jq2UM54MU7Cpjl9dGuDMHueI5%2F3hLMJ%2FT51pGnxM%2Fcgcyv3ysiKJgVdvlA093YCXUem86M7xAcCWRBxThXHbYjMD%2Byt%2BlEU9B6z2joDbi2BAPiK68n0GLX2qrDLvE2urk0ZG5lYZ%2BWhgwLQVIRBmRkkrE0ufKZC1HfLMKfGfQLeuLHVUXeeAD44JcJ6Wak5YdBabMInVq8YGOqUBwarwJ8%2BgW0%2FH3sFBcRQO1cUMPKVqLRaYIcBq9tiYQ%2BL24LJpQFd3s4TpUN%2FEYPabzxiZodRAIVoQciSjinzhlYwJFdawVoWSPDu%2BlqBfUvn1deJPBkmDWqYtC7x2lkffc2ppKP3H%2Byzymrri6mlMG5tzHZea53Z5YiNlLjrioGjY7i8AngEuUX8gZKnD32j96jLAntQbb5Ml8x64gJ4gRhk0OKmV&X-Amz-Signature=d22e0fdc070a8896849d82a5e16ebdbf0c921d42e01f3a65f7c3788cfe651a15&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

