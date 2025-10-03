---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYC4B6GI%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFlC0KXWScsf2Ze67Eph7tro%2BQc9v9A16eML2xBDolciAiEA3nhT5VCvVNX0SQYgR42bAnQNe6gLeaTLj5vsa1bSfmwq%2FwMIQRAAGgw2Mzc0MjMxODM4MDUiDBgwCWT%2FZ%2F%2FKzv2GSircAwQm5jaaYWkFtOnahW9kGcxQPBwIoXsC8wnwVn1ISj6bHIKkqXt6QXbsYsGf7YUYM2WlBteO9My%2FjV%2F%2FP38jWLMR7sW4pyJaRM7cvT%2FZdG3hZvx5crOpwelETw9LYorNgCqFuafXUayDNPlF89uOy6%2ByWlh1mCASMxw%2F3uuahS159xSyragckKWd8AHoPnmsudhWUo5880jlW5zCaWoawQyUiz561%2FCX733tPz%2BvT%2BE0xW7EVixoYoKlVDZCFSyubok74YyhjvM6M8ZNl9Fz6hs%2FAP7BNiUprNBWir1bEg7%2BVyoJCKb%2BWx9PItctdUp4aaq%2FX%2FW3syq7uXpq0ME5pPfnCScB4rD3%2Fn3lIvayjtkbB6GZiciG08LzStnx4GM3xKcXhZj62WTKmG8EFpk5FQoS8nGNYBqgW5s7Zwb6YR6lkDVysiUL7RH33h7tOIH%2BEnjwFq4GsMG0BYy7rNWe%2BncDAFZp2behldogc0TeQpvu1%2B6Mk3imFJIqVNMVwa%2FUJpEKuPX6ubjT0I%2FgAaISkMuwxy%2BWwe1GF8kE6jhdZYaNZTxFVNIY%2FFcLPBScGoFLADrGd9jSn1h9PkxgOD45MbWykVVeW3jb5SlO1NC69LBcAnplksJ0GGb7wdt7MP6H%2FsYGOqUB4OdiRArhAKksy1f1meP0ytJ19HzhcCPaM0cc48PchZDsgGSn2%2FLSMR4Pr8aaZX6vArr2pmQ4hq2RbCHkKDK2MCYB1rhSGFCFeuBdJPO8Os3HazvYIUXXr7I8%2BUX1lYX%2FN%2FP0ACvBnQDwfVJ4Skb2%2FdU5Fe2C%2BaTk71nrHhfZN0HBJqTcjeInkUo2jlt1DEeyw1L8yEYfVikxBt0IpCYA6zVdZPhB&X-Amz-Signature=2c175a45258cee132ee3fdde442b6ac90bfa11e2296cd0120656de10162a9b81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

