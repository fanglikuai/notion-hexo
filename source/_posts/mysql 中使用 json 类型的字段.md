---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNXKRXJY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDJpcMH3I9EdyX0d2E8dPmK5FCbE8ZS8CmhVO3MEYyu1QIgBFA2fLorvOVqWodaBH0CwsS4mPZCJ9%2BxkapyzCIGIjgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNxOhuOHWCtrfSlWSrcAzpYjmvRCIzeQrH7BUReOzSZnbf8gdmbYCxf1ul6wq%2BmOSY%2FN4aFeCqBQP90MBIPPt%2BZ8V5NYSMtajOxq1McO4HiNROUeECFD%2BMjygtUksjEfa1rv%2BzHfrfKsCr3f%2FO0%2FRLhtk9vt9BrlZQ1eBpW8WBZVf0gYXIG1j4axRwcdlvzEEVBL5n0V5NsmCghThCHYuFf4HyOtkezsQ3xbuzC0LoRJ8vdScJusb5zc%2BaOezV8fvdhZzsB%2FtZH9BTqK0%2B3xlFYFX45W4nsVF2rmphjJldeOwSq67jB1kj0HAdnyuly98KHbs7iDHjKj0UNWew8XkREQ156xr2ObkgQk3L0lA%2FldIaz3vWxq0JChFUM6DqWu8FlI7Ub6Vb3yZBucPLUc5ijAoRI3ogJJkGrLiCfnZ9FaEEmCEflAP3%2FYxe%2BfQAs%2BAgg%2BKYpf4bQx4l0SqjH6cI7LnD1GaiQUfdAP8YgapawNo%2BZVbx8g2RziaTFf5Tmu7XFJIPsRVIf2bcVRftup4fPfUY%2F9yhz18oxHU%2FBPCv9KTa6u1XoS%2Foy0NtUy6RcaSdZPoePKOzurspSUCumzUv9fO31zxWwM7JV%2FbJNSoOqLhgWohlDmOVCrmuE5cLD452QgOIs7IZWWVgSMM%2BAw8gGOqUBU7fdCaUgm%2Fv4s4AVLd5iMIrghHf8xVjRfWl2g5pBB3dxYR8ByAIh9OzqXbQDJcv1bwZGyHVWM%2BqnBnHai4PczZQnPezjYtivLzfZ4W7tXPn6HXUBQYx4pFh6cYZAMwkAXOGsft5ImWYKAWg8lNmViuk61%2BQoCrWSd2EcStYKv2VtwaVr9iDBdjgt5KK%2F2p6w76%2Bf3Hp0m79E%2FjtJHQlCQNxny4xP&X-Amz-Signature=e13fd2ca61ab0737ce3ade5b513217cdc39b22eca838d80b4e55e3421b5eee22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

