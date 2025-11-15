---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OHAF2LV%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEst0%2BtQM%2BS3fT8zJu26zsHb90EhZdnZXVpTGuMoUgSaAiBjret5OqdaASgqC1siMhh8uDxbJCEhSBiK9Ia8wm0sayr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMU4w3DjOsj5wQEMHyKtwDSDAOkQLircbIS38fG8TdHJEXMuZp1d4Y%2FIJTAqTZogjs5bv3Jyji5jJxg0NHr%2BZkWGMHQvJbneCE6wufqBHlxgiL%2F8EV5J3qL8PI4jn21qjrxhv86RkSLZYUXiRocWPDAON%2BPOqKkna7IkGW%2BLLuxl7qJceyrBOvMiR25JMibWhTOEeYpR39lEJsBTR5v7P%2BvFY1dRU1dCD%2F%2Bnt7sseb7mqzfyZlOxVP0zhFxYBJxeqvFi4dGFeDKoeMiv8UegTpa3druo6BDAtRrOawagsoq3NCZ5FBQJ72T48GLmHJ4dMIIoemyg2nnlR6%2BpOJGRK0%2Fic19H4%2BzfrjM2wNsVrddRzOYLsxtUdYdPPfxEO7agX%2Frv5qJHh%2BzdhWeoX4%2FcmbfwKB2tzE4WOE5R45YbA4WiplaLLGv44%2FARUwhVcaludieINXYD%2BhtHJhwA4LsMDjrBgamWc7iqdX0CCnqsJoXlUnhJTgHn7xQG34BfeXEMHFcXmrOfFFciGa00lIwc97QkeoF9QHN5dq0XXEQ4702m8fNjPdnr%2B5rxktbpVeA6ErRdGLk789Qsumj%2FeXAzaZBIjs5AjylZyeBQv4p2or7hgElvoaVpcH9O1c9pgXICSsuKQohsxRl9eZiuEwnszfyAY6pgEVfgGN6mbrK473ZUQZ7BL6%2Bnc9TAbX%2BDzw3jWkIuzK36w3vpp9A6LEKFHwjZnfMkxvbeL%2FllKELccPTt8vCS5PFK8z0HEOa%2F3TbQ7UJiaU3rajOIaYuvoDZQ5X1QhEw%2BtACX3vWp0ZDS2%2FDz0eOYpYvb4LmaY47Ko8MkTLNBF%2B7i8%2BZOthRUefXZoccYn4fftsH2m%2BV67%2Fl0GbeRAzLfZjRhdcliTJ&X-Amz-Signature=91a60547146918159510923bad25a0b6513e07cdeee58294635153102177daa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

