---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WSFEON32%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T010059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIEiVEbIJ7uBxEG0oFmqaLje38UdN9QTF8%2FBzqnCWTfsxAiEAjhW6n6Dw0FmRR5cczpJgdHfrsu%2FMrzwzo4xPlKqiS2gqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAnGuMumpixMFRz2GSrcAzGgYKSOsOLMbWgtk4iOPeulDV%2F814Jgz2xVFx8Zwn6jAmR8NSX0LFrr5iKytpTpotn%2FKRlis7E21%2FFkd6WB3BkgPVDfFRxRu30b6da5o0pzOsbTWxUoaL9%2B9xks8ePO51RH%2BRaiunTtm8saiU0Yp2h9gjC26xHPtEUFuc6SIymcwkYFbRAyTiYf8uesixMwjZRs0YZPL2KWRBp%2Fh8qeLFOg%2FCdX8WGk9yYyBwLAJxGAhIyQ4%2FQDWxXHJY9IWz9p2RFMI7sLWRU2ei8QDoy3UiT3OB6%2BMOXUAlzQ9rPk4DkC6FZQxmC1d4obyRqvoeuTJYSjaL2GN2qd4yfLd36xqs9yNulq1THxC0nbfnNbnznfHsPnq9W3pwqZkH3XpD90ArErs4O1Mdb2lG3tAsHjeLcb5mVbK5LGkshSW1ZSMRSEUSV7%2FemjZSSL0Gy%2Fjj3c5B0uyEW1Zhdqf9KcUHfKwyrlR4DCxkGxuvys8fm%2Bi4pdeusTfP5m%2BaTIJASzGCUKpxoo922nA8s7gFW%2Fr4G4%2Bt7vT11UaJMRjAN9bCHV9M7UuQOcUuJEVVoqkuTvEmwoYv6dSaGBVSYn8rvSHCuQjYZnkkNXNQnHdI5YlWKplRoxEs5PHcpEdiKkfA55MJCW28cGOqUBF7YhdDepcLwT8D%2B5CtFC9NmmvI5bBgLjGBsEp4n62hQLnkY008r272%2BXiT8dvfT5kRkndMTZHoSN3atsV0SCm42n1uafcaI0OnENwvbnU67MnB9Qw5wm%2BD0qkvuxcSnW%2B7sr8blZGPjQauRyh0Ryq0hW4Yf1zApM5GeUqoIKLtzk6kqy6tpwlt30X%2B6UD9BDyczWD5eQ5L0BQstE72MPkuLP9zeA&X-Amz-Signature=07ce43832d5e6e52b65ca4d67514c8b090080c873d59b16705d2dfa698fec875&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

