---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ZW74EFQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T140057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJIMEYCIQCNRnl100aDxHM1t9cnWMdFXaHqjYVlRQzvoOEhzdJRUQIhALcn%2F1zbz7PDXLEfrAdLXnu9X5%2F0l2Cory9QLzvh69%2F2KogECL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxqekgZj4EWuA0B2mgq3AMf0nMdD0qXm6fCDdMzb4ResG9xt7tS4G%2B9Rj7UHV4ykobRdQdFwrdFskk8JFZBTPrdxxQn5OCf29pV44mN%2FvWXMj0ST9EUwqqAtcxM%2Bj4bJ4%2FEAKgihpnP%2Fa4QU4x%2FROLnBDpW5z9TueAElwfirdAuZtxbJY%2F1IOpOzX18NVSbNLfyaqdCI7eMbVzNuLlqQ%2B4A7%2BFiwU25h80yf%2FA%2F%2BmRm93JRpO191Br31gtj376uFiAUdzzj5qwZQbL%2FMPVOC8CnAw1kkgwrkd7aqnfGimzYsnGdXnAYbQHGSOFhsDExLCFCNmtyE97QX6QIEdNR8kspwJ5%2By%2FNEuDqoLJekBh9c%2BNHpF1htHyqburbObxlakOztRn4v%2FNY2cJAchXObC0emUqa9ABmi5k4%2FVYarXA7en2cJucLu2I8YQqQlYkEmvtVX37zfDeZpfQ6dUt5g6pQCOq6H9k%2BTbv5k9We9wgPqb1ohnHISIpfysyleUnolK3CozU0Q%2FL2QIOB4%2BMyYb50aFpnj1lpi4dUfXn28Olbi%2FY5Tfc7%2FN%2Fb5SAzMac4kY2XpYNAB%2BAFShicQK9CD5wXelbm6MWcR3a%2BkiPHC6vHxDAjTxYo1a2hexI7A4wVKa0f1aiqyN%2FVOcFdCmjCZ7%2BTGBjqkAY9DqvluwQ2brKh47%2FwWqTJf6F9FDMw9Z58hzwHp%2Bs6VWjuXqS1ZBZmDtoYhQ7GBMU4%2BDC95SK7%2FvnW0Bi8a%2BpJRi61AfqqUIjHlXrrxIfPk2htLRyo2us8xlbHo7EE4fy6C39q87FXMfEUtMtm2%2FpaOLdtEI22O%2F9Kbfph%2FfhFbNa47eH0VmNav61TpjpAplsGLpIzhAzeecBdFWuSLsIJYlkD%2F&X-Amz-Signature=8285bc182d7f31953d9cdc44132d442f4c00200e0b866c6c448f0ea1bd30055a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

