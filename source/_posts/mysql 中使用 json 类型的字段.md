---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V4SUIJ62%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIFWmXh7DVIxRnqIFq70bBFKsuHGlbO2DvVR8B84zBq3fAiEA9YC4hBkaAg3AmjvMb8aoaT0agmWDeAX7rTjdHGyKcogqiAQI4P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI1LCHCGVY2Kk6Ts1yrcA9WaPn0F75KqKalkn8SoEWQRgveWH1GN%2Fg3CZNeyArNinzxqiHXaXY416zc8g7yVpXXYAOT9MoC2bDTk5oAz%2FKmdY8cuNAOtG0mLN858uGs5GgIzY94YAg1dSsyefaTdI58ksov8MI1P1FEqNFeb9PMdvrJde5Zn8rZWDlpE6Q%2FAz%2BvK27NGEJwbUSYZd8ZvIxLJuuOaDHnG%2Bidx3ZMk5bS5UpkEKKYH1jcZ%2BlBf1XaCT1OaiCTvMj26wrMk51%2FvwPgcwEzmPCqQ0LR6tmJ%2FojFNxuDEtOePLLhQm7QZn5pF09K2FYbyUP4WlOiWecnGMGb6%2FGDsmLWNl9SgnY%2BdPYhnYUtUTGf8C%2BXrHpG77On969UIB%2ByB%2FVRjRMQzSU9ysLv15DMbOhaeKZsqsZ6aSS39MFcmJZYPYZJV%2Ffqhm56%2FakE%2Bn%2FPmN8eDlQ%2BsjXvE3CFbtvF0eaTPgYf2aBhg%2FHtYcURXcL4krunqSC9PP7FostrNTh5gn0Px%2FAN8Fi8I0M%2Fdl2pjA%2FqaDEu7n8SNsGsbcaDHyfPvck5UlAhbeMBS2xqjAAiyfcUOHKKIRZbYzakBR59xBEJefud9kC9gzm4%2FY%2FBFOUS882%2FB6IQ82mBByovTXvMQUOSmFlZ6MIy8isgGOqUBvFxJSIoYJWYas7Ewz653HCycOX%2F8uy0IjxFddrb0YLCY7lLYnX4vuNQd9pNbyuTUztME8Ur0bXUSniV9HxUpw%2FLISStuL7DNQ1cBsA8UUGnRzieBwWt1JuFnr35BqCueclVeZTm5tAHHtWVC8i1Gsd04J1W0AG%2FBWWkqbxxl0Wvb0VP0nCZescyiVJ97e5jUakj0W%2B6KftidwcWOkt2BgaP8kjaV&X-Amz-Signature=8ca10791fdccc3b5b8ca63669ff390a7f08aabc62d5180e2a6fe88689e2519ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

