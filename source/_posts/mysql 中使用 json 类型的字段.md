---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PIL4PDA%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIGqsv7UxopkPJdHPUfG3PWJL6SW4m5meTAL3CEPm1CwgAiEAprzKv0jnhVyeMlRvX7N%2FslEzoigI%2Bivlr9wn8WvJ7Icq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDCmBp9sZa7GpA%2FpIZSrcAzdipK2kW06doZRY%2FHTJhk%2BTPSIZKJoAQrt0j6ww3utHGlBa5jGMoO%2BN%2F8OQuYh63kYGk8Bvy7JaL0oEkza%2FJAfBtbxh986yF0MgotNGOqvQmSBoxunvIRq8pFQCm7xrMzjhC5wVJN311p5KZWm8jQ0ULZ%2BlmomzT0iFnuD8I6z6VgNhUjaxL5EjajJW%2F9F2NarSQyMzVCcJNykbfh%2BHj46wAGspJYfUFrEslml%2F9XdoFBfGQWt5jzUfC0uYIIieVm4UnNgxzWXtgTiXD21fMIiTJuwhEhzBBlga50sKnGh0qwwXCIQiPcSYCtMmEoEHCGPjVWdPU%2FykZT0oVLz1MonjfDRaZhaa0XRX%2FOGuHIFOZzaYPuW23QXCJU6zy8eBO2FVTyHGEYvvx5%2Ft%2Bw6TCm3DcM%2FJW7wlBQ4u3H%2FLxCZz%2FSOYl0rxBkuCLEvz%2FpRY9FCFKj1M15a89zwXOzZaVnsA9cWjoHKtfWhDRIHzH%2FxJhujB4hGv8XDqnY%2BRkYaX5xm5ZxLx8%2BZzqCDOcF03OB6Inr%2F8hKD5ChjzS0by3oPugsrsfO54RkF0rqiM%2BLQqomwDbjAdVujznH6r%2BuoFKxHsGQJvn0LevXMg5OrJ2fujpw3xgZvZBg83H3eDMNjx1cgGOqUB7jmse8s3G1tIS%2BNLF0UHgcfv2EDr2b7YsDeZdZQiUh%2FJe6QIYyhO6WyaBbYz2AAi0BA2lchRuwbQp70d5354WOLPMsadCN%2FDnV%2FV4GS%2F1Lm%2Bfo6CNGTDH3qgW3oJwRuxASln5kmg3i0z2JXgHawkN86lj0ESJeP6auKJ%2Fc8RelCb%2Bu0emAklf3XMV88CO7cxIlL3Fc8cG4zpn91ed79fEu%2BhQEiV&X-Amz-Signature=99a79125669c1754dc954987f2837be731a6c1e61a0696fdea6083b352afbaff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

