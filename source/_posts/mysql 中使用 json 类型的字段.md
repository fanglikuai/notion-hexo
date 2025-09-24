---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYDQEPZJ%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfrDUH6B2vQZ6hay7feNn5b2x%2Byt3TwSdJ%2BQdiQ4OnmAiAmeZNelP2HaoNXMFCHKyuT3mC%2Beoqt3EzGUxjaafMCqCr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMJP2yLvyd9R3wSwKxKtwDT85%2Bv6ESD9tg%2BM5fFI6QFOvJodY8%2B2Fhn1SLCC%2Ba4Kfak8MwNiT8kHFwIqxyvoWdJUZOEayhP6E2j49bf2lywzi6XATwFbH6Us9sbuYuCdJ3inuTfcWmO1mW7WqG6vqbOUwfINmJ8dde4%2BG3IBkVDq%2ByH6Ul5g%2Bag420pM5GeYYFVTVFpY9RBS%2FeaI4FgDJDGL1hDHXLwozvK3hOJLSyjH0%2FIlRctn3R3cdmUb9RxWgQKi71NoCvBs8fk4L2hqXgFXoGKsmcIAle4DHmNnNYco5lHi9Gv7moiRgyw%2FIZ2nhXdOX6DasOoW2b4hNyP9opTFh5QT%2FGxQTMc43Lqle7IdVZpem4UfECQDk575rJPPQEVK9gP2sljGi%2F2y20JVEADPPl2PfSRpjjbxuFBYeQ1nS2cMH7LeaTnkyQiAvmEUDL%2B8MdghQbzaHJkXyarW0aA02jJ81HOpl1nOw3igg8DhNvFbPwu%2F4Yt5PaLrFYDa3NJPSfavOYCLh1DJrlmQgv7FXaAt%2F9e0nnHZOVunFqbJ%2BaOnDx3hFYdEz8UrWUWRDP3Au0J40CZYKbQ8KqlVpTF%2FQkg%2Bvh0dhOOEMRQsq%2Bp%2BPwm6F9NzyKPAQrBGZGVA1pcgZP%2FAMmdv47CDswj9POxgY6pgHdelQ%2BJh8VVFo%2F46BXMhEooL5pXvoNJWMt59hzNsVAJUOsozYMdrBcWrIFczrZwtUVrPX8k5iPYR6%2Fjl2VovmHZ%2F9AmF0M%2FmgJoaBfbEvBvdQuq45cRLFIxEfC%2FYZbfrFh18GnvbQDOcD2qaoq7SD074poajvfsfTkUmp0%2BNhdKr6hXt81jS1nbqCAw8HmJPJkgLY2fn2HEoVDpx7gQJqK%2BYXBrxsd&X-Amz-Signature=cedfabd2b85aa9e9d4fbf92670f2c953527221633abd6edb2cff35d246beb869&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

