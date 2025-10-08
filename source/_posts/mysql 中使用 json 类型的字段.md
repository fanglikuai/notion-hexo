---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6I3GURP%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIERwH779b58ErCYvauTKFqW8dSQL0ZmL09gZlfhINrnUAiEAnT%2Bt%2BlaIuaC6czNbRKpAqr2bk15goijsrR%2FBsTdh6jQqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkN8%2FXs37OORz1tlSrcA5dEXsx%2BhmFKx7bX9H3E4ugjAKPCD2IQ96PTkCpIXF53mPIerANPQEaRzizGYgMNh1J72ojJLWf%2F7tYVakcy0BtOU6KSJb5WaZcG9jL%2BD%2FxgYD9W4vu6D22T1yenmlKY7%2FEt4rRatamojO11F9xQ%2B9P9m6JmNND81PBOPtnzV4CJmOLb1LEyl0DLACHN4I49yjtCn%2B4DnPUphyErSh7ulFflJMs7ewWZ0UBY1NlIIFlbdqiXwlkGrtKLzN9WHXtCh8Gp5e5SKbI2qhNFa%2BZaGcuiJG%2FMixFspy0jcTIywhu3RvoMtbUNA1xU9qCb57Fas3CivJoEELx067klc5xXwzIu3Od0FOffGmneul5YwPHu3tFNZyUtIgCwugpNy6mBk27%2FKhw2PBC6uqUCeCxsgyakLsGdcKWdxFz1Gf%2BrzCOCHkXbDBO9oVjtbGmMvUAyUms0ybyWrYQAUCyI4ZbM638FK5m34htjQNk8YIsGTK9o5ZxA2M7paupeLEogW2A5BTIq36cfG7jKkMvxdwrx6kjDznLqoMB44wALiV4ucl5mXj%2BHQnSWIbYK96vbngEi8sUrAqWanmNzcnxslYnTZqj0cj6%2BUXDAXr1Pa%2F%2FjBgHGH5zFcopb28S4qWrjMPzsl8cGOqUBCBf0EJIM%2FyTxmciZLuPEGGmQdCSVEHdGictw8JjUrO4SMDkGBeJMeIhYfDGXSl9JDfhNfws37kZ5v%2FXYwouww%2FwWOsutf7dBWRm%2FPkqpoXxqNe%2B7%2FSJdJdklZV1Y4BouiXHKlwMwLhO4JydFCl%2FEHfUThXFuOhc1CGUkPrm0W82VZSxtu2upGg1okyFVg2F1gEe1No7fcP2wIJ0bBC%2F5YwI6sLjr&X-Amz-Signature=a84a2f400a4bc496bb1f0ce7e3b1ac4d6b39f0ad1b07949fca9898712ca5b6d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

