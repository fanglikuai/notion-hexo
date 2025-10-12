---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627UDHBGU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE%2FIIY9gDHoxGUNTgROBvu4BdKZEeXcYq%2B%2FyPVLPW9VAiATCQsD9PjBTk%2F2VbO1O687byfJLnsA%2FcaSjYY9L6BShyr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMYo6uPfeYjrQHMMs7KtwDE%2BIN4516J3tSdr7q3nqKmhyykNiLplufmTapmi7sTgr6l8MWE0iJ4%2Bt1nofaaGTI8m9YojnwUX%2BwkE2ut2NrwEEYsvLjWSY0woqhXdWe%2FWXFFTVIMq1Xlg9PbuXN16AUkVGNB9%2Bclvz%2F7J9defYvp4pF%2FA4AUqELiNefckQcotUv27cSESbpZKddEFQZdqq5kL2kYr%2BC6PIOLpgl6Gl1YYLRd54FMh0lkxk%2BHlJ42POFZ6o8tm7%2FuKvEabuhi%2F2DT%2BkIuXdg%2FdLpKxpQ4ZwxMLVUb4DLqol4XohQwkM56QC3ShTtKP1JfuwGAU6kI9BznIB6QJrZT5dF6Y%2BP9mqBWMc67Zl%2FOpjq%2FMTqjduMHdEmZmWQVUsjY63VeT2DwmyscGQjgwm%2FXcldl206ICLMmTmQ07uHGpQCZdXYqMA7yqvdFG4Hzs2X%2FCPwJ68ddiFA3PoDB0z1B0w7Ectljirz7FQMo%2BzlqW7VcTD0OdjguT0ijwgQ1P%2FDhFLXbgeBqMcA%2BD1uEQgrf8OBetYRRummVMczGpdmGshJBzPQMTBQcHy1V0dBcrM%2FqkppbjY1DDe3jOfjrq%2Bhgio1HTY0p4PTPJrHrvytf37yMnA1LshVqJZaB%2F0p1XVMEYbsz74w%2BLeuxwY6pgHRnDIOMbrClIhcefHjX9jlbg2B5kvBNWwfPGpQnA11JOVwN7n3PAZrEGD%2FP31a9q8BnnCdjZFG1R1v5yIqk3kD5uZmXrYwn%2B5tDlVzd0egRepYCJW%2F70ADabUZms8%2BLAERGxfNS3IlDW8B3qtLd1mHQsRPX7AOEGqrt4AVKyPfm%2BFmzxELS%2FeiG5hg%2Brqf7u5gRDNugfWZf5yf3ajBUAUP8Snl9Bpk&X-Amz-Signature=1da2fa8b9dd60c247fde322f583b426171b5ac884e3371ff44459c8b970002f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

