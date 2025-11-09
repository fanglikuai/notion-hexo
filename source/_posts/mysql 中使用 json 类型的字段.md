---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCQESK66%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIQDtKNJdS%2FEkQg6PqUDjnoeN46lLHTv0dDYBeOEwCr%2FMQQIgLQtuR1wVc1R8ZM2FNfqkzbkIy306q89TSptau%2Fm6o70qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD1Tr47Z6sVrj%2F5CxSrcA%2BJqPeo49hZFow6kTSwgoqQuv%2BrK8l3T9uPW7K4PGAUwth81uYfekuGGKsdG42AwY%2FPVu3jWHx%2BjaocHku%2BYYi8YnArWtBWMWzMVt5fwvIAF7tkqg0vfmyK6Ga%2BOOqCzzfLGP7AIY3XueKK%2FGu%2BFFbjiD6ArV5PCUs2Bi8UWmVlEzWiulPRdmVtO7H35Lz0jAKbj9SMmvPdoxb9O%2B1BJda3ZA1I9T7IGECqhW%2ByoHPoozlq1TKVvVwLHiUz1GRiAJHSsXZL%2FJndZdAQ6BlFbMcZJL7%2FspCrIN%2BbtNdp8sz7Ee3Ikl7ZmuQhxa7pXEtOpoissDenXUNrk44DB6TfjQxcPfn0lRPbmiz7yLkXplRWNdiwoWbsTohN1wBgySj7en6Y%2BSwBtp45jwI2ryXU0zEP4Wc5mJhiWuOGJvwpkfTFxI2aTO4O%2FHeCDLuYQVkOv44iMeeSKtsaieHc9F13dHWUNNKE6gmBxpFGUlQph1kFpCnmoPVJUzyH5xz3jDbuNsy2XBt6iZGqa1S2f%2FUwZSkDOJcuAbiyYEmMv53CTulqITPVSbFh0cp4sxllXDAiRXOixEof8MQZh%2BxaDIEbqWjE9e1t1vszBG4uMCBJGxOJaozCCrDZDN%2FG1%2Fz7%2FMJXuv8gGOqUBg70e%2F6EF0%2FEQ%2Bu3wKeHV09wGi8kGtYuov%2Fs2eBkOu8HuPnoFqKvqBf7Y%2Bzi3r%2FHaCZ%2BVCz8vA0mTVAubhHgtkELU1X%2FTF3NwaFyNTapoav9uqqVbN8SvG%2FM76hh3ZDYtM%2Fd3kA%2FLSpiAZhUF5ljIijb%2FgBAKLV1RAC%2FQG%2F%2FEgtq0TawFbmw%2B3r5KXJ0CnpvRpI0OE0eNuJt%2B0VLNJlLDiAUkdG9p&X-Amz-Signature=f3c9ceae0833e3a6d531e9864e854f6fff4cea8c2ef46239e8ad5a6d5dca6ced&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

