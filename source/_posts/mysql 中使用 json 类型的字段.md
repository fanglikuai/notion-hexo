---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q65T7QQK%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T160054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEsRTfupXA2ToHU%2Byl3ezRbExnkrywn%2FU9u2ZmJ3CKeLAiEA9wIcJ5kHBiJdvUXEmtjt3B%2FluK%2BzgByM8D8eWckq0vwq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDHHqeDrkt6V4e0fuASrcA9AE0RaZ4FbcaIc3r%2F3Wpp8rWMd%2Fm8%2BDRYhIgsPz8q%2F8vq%2FtjvYZr0AaTiWfpLZ2EU6o%2BtrTKVso6cgmObJYlYEwn9cL4YxdNbP6R0c%2FcdA8VEHEUyqLQTkoBCOKXH9isd%2B7GnhWcBU7kgh1ipIQ%2FyOdvHGkvjGC8uB0aZgpJb%2FJ0GUeOcddjcucA56Y%2FQivkTByr7JmiBikIl1RHCUlLGVlJ0K0GmsFrs2zs%2F4%2FVechqa3kqQZXE0IHbOKceJGX8iVbVv03QlllIrVuQDjDW8sjO%2FfyAI%2FAHXp44s16EdWTeV4rO8%2Fe1sI0DLPTWvpv1W%2B0Sv6YjLeWFbD53%2FWYEJOhQYQ3J50P86315FvPPDaKu21FWkl1%2FZJzt4%2BMNx7IoBP2MnNX1LZm9ZwjHJaCrwkNhU3i6iu64ekgJGbH0dTZLPbxhuARX6QWe055aKkX6cluUctgvhNXjLQqd6aehAH5F615BWa48SeBP7WOS6yCKywOy3tYVlE6OXLk47h6dtjITqYIKjSSdrUD1f1FJd3of6W0jIxjj1oFl8jEXSgMasphxYVMXpjOpjHIvzZUZgzy%2FsJ3cgO5hsZz09hkb5kXSlbKOsNTdyW4abjxO%2BW6ON732U1FiGbSCgVpMNj6kckGOqUB2D9c5SGdDdF2te3I3ObsGZyN3Qdkk0mjRf0Caz6h32VKIVCnVrmFnIicRUurfFumTyyAN4nUKCvIJYYabN%2BIVf8Q21VW9g%2FObInT89DbPnwv8CL%2BWct9ViBvtLv6OWJMwWyb6FbKS2e99a7U1wVcYu%2Boj1N8Iol8tA6RoFucJTTkbR%2BTNu8q0Fwa3RiHbp%2FZcDpedsCarMIeuPzDq7xWYts3G1Or&X-Amz-Signature=b5905dfd8f680e43cf76c5c2e605a3bb19b8e7e35060738c198db38eae7798a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

