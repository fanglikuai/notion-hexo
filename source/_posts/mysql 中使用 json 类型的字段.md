---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UR3NWFRL%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDTpmFLCzB71TFraJH%2Fe8Sh%2BpY9FmeH71Tgqr5LPc1yHAiEA%2BAQI%2FgTCCPocPBVuAbDWqCBT5sGOoHsMlU3Y0I82JT4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDIvb62czUzuZNQ6WVyrcA30OUbv%2BzEW5gIw%2BITikcLUYLAOaj3CUFILcItALLYIdtzH4B1GiMBJkKvUIxsDj8iU73ncjwpQbG9b4xA%2BLhnc9r0FLDGaGD86i9Xs6o4T9CjSkFvraLVAWHxSCHwL9WObEr4JEP3rRz7o0%2BXOoL7zLxC7GH5c6VrL9P%2FQVtumvO1kOrT0OcgcSy0CJwBqyl%2B9mgMetGdGw5iBH3m3ZO16QLLVbFRVvVWJyUbdq57WvrRCUitXGLaJiIC97lzLlCSZqHzGly3mtw1frlfOVbyRRWT5irFqpdVA3gZtuOtFlfgRbeiwb1I1BzF7dKvICT7i17c7hxHb9H5QbjJu90VrVgkb9dpt6S%2Fw0unReZSabMB0DVVxCizyRFbNLPIw9OXjetk99eVj2Krp9Not%2FVyCUn4pGXHu1ZyuuW5mmzvM64eqcTJTTp%2F6aZlmBoFF3HtDkmS%2BvokOf4Tzs43NqK0AJ4d5UMoQflJmIkuqjE9j%2BVge7fmsWnLlRgoHw%2Ffnuj%2BWb6iB0hBcVtkCwdeqkBavxqr8uMkMrPs5g2MriM5kyAYqiSq0lXmDODftnPbhT9hjnpwxTXhEEsvwlwB63gBi5qf0CVGpZl2raOzjRTHq4RGvk9cNDlnCiIDlaMOqDu8cGOqUB6BbkekKcr0Juyuf23Vvs3h13BlVjf%2FzPzI6tmwMVN%2F9ivVpg8VSwWUfNVwh%2Ba1q02n1tK9SoMqpVuovVUR63OhDILHf2SBroo8Wl7SD0RBuATzpgGz1UwicQlkMXaveDpXlkqvRuo5%2FF7t4%2F2J%2FtS0uqUjYPVPp0LszfvtJ91fJqmhqy%2FX8qjuBwLLu%2BtbQu6GYOrQafD9Ug9zzqzqCRnRlbdR27&X-Amz-Signature=777b879bec04ff2a5d895e1e05eef639a45c3b0059da4d53194f996d94ed7d7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

