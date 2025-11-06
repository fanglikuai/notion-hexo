---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USBCQ5JT%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCzBe4gNq3d8aBtkpZTGc36xTc7bXSetHDOJ7hjCFvmNgIhANYknq9D%2FD6jTfiWSAyAjMm0gE5lWJ95CTbnQuMRXOH2KogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzsIpWXaJH9yfX9UGUq3AN%2FoOQBPCojYXOBHm%2B0Tfx62aqgi1Dje8K7GD7bjCXzij3RTsvyCclXEU2Eu2WSFMY%2BkiGMl%2BT5MPu%2B%2B3QzrZL8Q8e5vAQHUzV%2FHom1vfiy3pWWZpROpT4GzU6z1ntL6zoYltytvrV82Ok08soN3MBm1E8r%2BoYIzP1drtliopf6BSGr48yNGDw5sqNQuzj24maBHQM5wtHCOkmYhaqfdS%2B4apcTJcCqEW%2Bf%2B4tG81UEp0aUPSJhLuFAbNa3XH1e0jM0mSf4l048hfz7vTO0r0OgnSc9mWIQeYhftdM7LsLIXgsNut7Z%2BwNxL1ulNupk9Uk6XGoVTfTu9jA24FLjCstwSBuFlRvoaeOYorj2gkhmKC6QEycEzsEqolWzR4qREhNB2Jlue7GyBWwp9o1xOxYs3gDnXoaQPp64onTrMkxCSJpjIMklDi2IWnPiLeuPGvQfBgve76JfbTO2Z19MISBThyOSyjVy4WXg3Gxs8fHCsDqczuOocAddsyUHU4DEjeAWMFB49N18gxiArjOAffIwvnoRPCn3me47R%2BHw4PCJ1Fz8AuQ9GkMO8Ye6XbmUkvTQF%2FWRLlmZhdNLvrrL91WiwUjDJxnEPxW7qSEZhOr9v2jGOoDblbkR7e2twjCp5bHIBjqkAXvq6MbiwXQZp%2BxMvJV0mayjM5tLoWVm22mJkQVT%2Fxwp8Kzt2iuWay0NAmE0aC6ISlQkvMHWWP5m8KW35iStLCCyKs%2FyECIV0EUcq13ESd8MZQlU9xc36cqvv6PplDASI5ozPaYkXf16dyTaeiz97mJxM5TYQcjWLHhiagso23ywyIIXf8hF1TjSliihNBQxRA%2Bv%2Fr%2Fj0nkmgVNEyeqflMAixmvF&X-Amz-Signature=7f2ce4278f985edb712640ee0a7b51f6469011e8aa21afcbfa25f63d35ccc26a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

