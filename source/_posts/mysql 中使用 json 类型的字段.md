---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI6LMTX2%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGcNfWSZHH5ihrC6PHAxUX2pgu75vlI0CwKe%2BbmImcIwIgMQaJlXt5EGf4Pdqheiv53Od02zJ%2BHjk1M4tbD9VD4ecqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK797oZhWBWCDoYXECrcAzxRXQDuo2Er6QNdQxMe8cA2tqIafZ4pqI6ox958dLYYmVKOJkYy3cGmfh0uXSBwqbErOqp%2BwKilwHg2tDvhjwsO7nmqx0HlQIV5yvp5PIJF93WEk8DGjpmODu9OZwZpaJEcBZySmEEZfC7Jgl6cVvLNEDD1ShLmvkSGPgR1vGggiExFJJ95RxLiwSN4jWED36JoY1xKWZt3coziwT9Wrl1EGAu9K2aTcH9Fag%2B1S9J916LVOSU2b28brYKyh2J7A%2B61VH5wSV4VVleDkN3A99rAbEeF4LuWEbX2IUM2CO47dlbw90W0LEB4O8Uew1yxIT16Y0Sf9FBhy4B%2FJFrs0%2BR46Wl7Y78xcEOi3fURO%2FKNrnbDACLbrSXHiHc646zFtyCj%2B1fvYUGoLyqENG%2FnXm%2F3r8eCuDpRLZRyAc01%2B5OkGcwqYR50%2BbYxDvlNNqm2zYXlikqGEoDFqn7dhn00tYUfLWI7MaFcBywjcDVfeXS6iMpgxWa8yngoj6AZ6KtW6NBQaQTzV9l%2BNHuWKiIOSse8%2F9QbPTMXcw9B6OY4%2BzOrK3wG3W4ck2UHOl5tdP24zADvxTBKckQgVq3Vi40%2FNZ2xW0Vf4%2FsTBTrFb2CC7dJjzY8dsrIRpnJdpBVzMIrP5MgGOqUBEsGGIfesnEHo7kZVEICL%2BmSNNF%2BKGe8x9NxlQ7DUCWT6RygNpESO4OXDyAWjXrc%2FFMaVSZkzPOCor5bfFXcAW2EZsxQBucrGW76aXcPfbl8ziGdnyo%2Fr4Nm9JNm1%2FagdeLmbub83847hJ15ospCCB4VpHUrlubIhBMBG6ijg7EdO6r5jyO6gWneMVOilu3IkxbLLTpPNNqba78qUgyjEYgr3TKme&X-Amz-Signature=464cded9f6efce9a48474c4c1b851857c2fa5dfaa9bf6ef4486844ed5b2218e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

