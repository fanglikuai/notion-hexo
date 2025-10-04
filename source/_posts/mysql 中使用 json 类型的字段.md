---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PGRT5I7%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH6y0JYeE2wIxIFUwk1c9ktqse9y%2BE1woxrDSYRbu6JmAiEAhYAWgA7uUMMFJ6viZThowpJF8sr8hE8YOCgOjBFX%2FNIq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDEdJKeE31hj0NfOsfSrcAyQVhlkJlmAmOM%2BHgx%2BqAAuUeR4BlQI%2F9KJ6qqnBmQJOYPdCZQx9cN3v5QO7DFj22kMThi4GsL%2BTUgzxFj4XvnLK5oZLgnt22LKhapFvYsNRC%2BwbOIgvWyGM8bW9sTJtYRuNMeeVSTkj9HtZil1JxYbR34KFpSOAbL6gevEgTQp6cp7umH6Kvz2G2rEQYNSnZOpBBNP2nEZBH2S0K%2FWlsKhzxm9l615GvuVgr%2FUJ2VkJ58F9iMEJRfAARwxSJbnfPgUYBEat%2BvknwrvhC3PiSUwAnH77ITr3a3TsH72ZH0uegUpbyiWnV4YojlYbpekAjXx%2BDzE7fe3Wbg9AmYXpuD7KhiZQO2IXu1SnKa7S2xMKcqKT41rnECt2NUhbWaryStG%2BShhs0Xkmxb7jL6bEUoCkVMlJzYT2X9Tb16RqGrpcXd%2B7boqiq0UAm4StffrS5so2J9%2B4QQcJ15yqadahwBvAwoH1HcB2w5pKPHiGFtl3ZtQZjNNyePUhL88bfAs0lmL8xWO1i%2Fsl1lyLL8FtBiouMCx9I2a3Nmar4Q%2BxIKEjar%2BZrP8oYkwe9wWUDICmk2Wpy6ZPhBr%2BqCDrn%2BoCpjgJEpTo6l83%2F7WAd2TnO5JJH5dV0VdLUtRUGB9NMKfhg8cGOqUBweaSpVrynROPZ%2BcXkITv5f10r6YS4I44bigaFtS2hQHPXPxMH5n94xIpWFdnU1eaQHUe7SOLn89J5etyoGHDDTriohRBnQyPcKxSTspr%2Bc4BF8dZfZHHlHl9rZPsSj43u04J%2BeNaKbRkrB8JR1wUhAExgOAqp0TuACsxfDZnJBUyg2JrjBfimU0SyK%2FCAgBGd4OR5aQgCvfvTNALxoF0tVkvJtYm&X-Amz-Signature=be35d239a49a110ac8664d05706f9a1357fc2b6059ea9fc94519f0f6aab055ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

