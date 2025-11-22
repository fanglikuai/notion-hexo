---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQZOVCAX%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T050053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIDxkSOaoGPWetFSBnk3GvPYR57P9tGf9uLC%2BAx%2BSEobjAiBLY9siAODTvtcYUZYrNduPfMnWfCk%2BcF0Dg51x4ClOXyr%2FAwgeEAAaDDYzNzQyMzE4MzgwNSIMQESNSsrZR7i3eje%2FKtwDSoyVsKnPsTc51IzwtM7yzq3jCz1TP%2FO16A0RC71OzDPisfQ1rIEk7RCwOlBXsmyL2NBDAfpzt74JjKwbOhxWFqjzCRAWZq7jd0U7uS8kd7%2B4WjwaNp8qHS%2BCj0LSLUaZHAEFxYZg2GhaV6GiLQMxfMkVcaZVFzclLqK9qiMVETyjZkQkEeWcCyWHykRguNFRnFt41aYWjrDNYgufpAlk1cV1uKxr3WcIy2A90WDv2mVdM6ALVCfEDEiaG4X%2BA%2BNmTD2ZvubSy12uSSHHWiLR7NEs0G1MJkakRXWGBbSlLxmKABz7%2F9YKOfYhWIjrLAKtq%2BVeepsDTRrJVnKtTyYRm1wmlYsvMqe55D%2F1xh2OzSxR7YHysA8tk0o27KjK5OUc%2FUiKvWfEoLQKx%2FTA3wZzVDRczu7i3r4HrkZ3pJy1vJxkJ9eq7HJai3ukAY1eJ6DbbcI8IWp9SoGOYyaA2VPk4RrA71kR0qDfp7McfPuSEmD12uR6gsetmmigkv5p%2BGblPjkJlto%2Fl3mEXACgawOaf44%2FPUctQP2XC2iRUclPoC8iKznf2HnTU98j0QAYOjzc0kqTcknGzRfwZ%2BCQ1K9j3WAq%2BM%2FbkSZmrYt3DwRBiTmG2dmN2HwTmTI7Yg4w34OFyQY6pgHS%2BCmEDO6sIp0sixz2fG4ifsjyWwQ9bYuvDeMBVo8diKbaBm952UvGuqtmHwx5beW%2F6IswlgmNamjqbYTuF7dDN3aI6iZp%2FdCzhfohxRX8zQaZREOCg%2Bfl3YcwEXL4zLjkChE5I3tzPbmgVo7IjCYjYX%2FQfvXFRnwz69M%2F9e9Q7ss6tffyGPbNM5HUvo8f1Be9et1WofXHWt5jwNQA1Br4W%2B89g2%2Fd&X-Amz-Signature=24231caec5b87da230bbf50b07c170ce3535dd28520caa1fc61548cf10a8f31a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

