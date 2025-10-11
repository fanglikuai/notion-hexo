---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFO2B5FJ%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQCNCH4KBfgYDoHuoVnL164xxk6Fn9fJMSFA3rPd7wqIEQIhAMRQwBJUXw4367Crd8vzn8pcScOMXsr4BiFvqBainj2SKogECPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwsOGqC6nOHBcvgZlwq3AN3gJ0it9FcEOOz6Y4LBCdqWcScD5J%2Fnhoxpbu8Iz3BrIO8NLEmXatpOOmPgbXscXKK3dQvR1W%2FN%2FAZvBziXgnBjwBppb2%2Bvk9MTPyYkfVjqlm78JkkVjyb338wML7xkamcKQ9Q0nkgI%2ByMkW1I5HG%2BqJL%2Bk3TXCqu7bOaATZ%2FiRCgUYwNI4o7F339mRGGLrlJAogifGq8L38zcLabgx5qKWnouyMdbb%2Fn0d97JtvK0yevu74JTYqe1Ejsw5YII1Q%2BWwp97kfNkLm%2Ft82eA2C3KUirV186QgREb7bZbPtx7LFwmBaJCmpharPDbfQBKYMDjwziyVWr09nZBy0%2FaE%2FTIddbFZ%2BXtko4O7MqP51CRMbhUUCkImPToTYOzuc05h3zXc%2B5KwSwdehEoZUnNOEtP9jJ0fL5N%2F3S6%2FtWHJe1lFpdEwQmH7cyXeGX8xU4eHft9lTePhpbiFXysfgT7SFVAaVQ89qNd1hJCduM%2FW3cH4yF9BQTZF7C6C%2BxyNyRcOe7emBF5qgArTrwvxqgd9q8sJ5TXy7%2BAi6Ww2nibs2UNa5eomKHLNEYyxsEBhtG%2Bqyn3hHVSbsafOVjLs9q9WW2SISnzT3rs9figQVsn7TxYXph1CLb1FU%2BvCXA%2BMDC9n6bHBjqkAdfE6N%2BK2zT75ebaEaQJrx7rC%2FWSspFnx15cA0SJQlfJ0QtRfC3EYb7nHh3MPnhfZ%2BKK2qw5SjyvPi2ptlrL8xGQeE8tYFDbb56zwEhTyfuVgIKcAcT%2B2n%2B%2BWhehs8y3yP2Hn4eVuFb8aNxSRoCKjxrHfDiwqksgMebgh5rP2s0hqv4QbE9gvOs4KA4rUH9ctJ32ilhgN4222hcCGXXF8%2FSHcWP%2B&X-Amz-Signature=f38373d4fcff28cb576e5d05d932f8029f829375806f2fbcafcc63285abfb2a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

