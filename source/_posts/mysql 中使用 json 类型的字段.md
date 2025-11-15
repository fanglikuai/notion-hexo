---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632E5UWOQ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC8u1pi4EXMJVu%2FpCqbfUkh%2BeZSKcj5qZ%2BGc5CzlYVh4AiBzmpayOdKQP2yGjgfVGLWAgPyKpwHnVuZOXoB4BsasXSr%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMUA4M2GKqvp3JhXCZKtwDtZmuZZX%2BjZJyezPKCDcNjtPe%2Fb2dfDusbm5AUpHokfUMDDVwmFzX0nAfbAGy4s36B%2B1AD899ltbK1905B8CXyNEyvIgT0wqsjYuqLWvDpwozOrNAJ%2FxASwyU891dS3P2ZoERPeH2OllmWiF63dJOXibxLneu3YGFRT3ZFx6QaEnX89eE1T9PIU9AMBuoHX5g0gFnYW4bz6n0Ogi1izQrsIc3%2FeE5wi7CNXspTQww0pf%2Fgwny0%2FdQ743nqbeSBh8NfM96Lp9NaHy4JF6%2Bw2F2kwY0mrylrCacWi9hLS1F1HPwOgfk2oQy2%2BLeKgFKSfwxaaLi%2Fpls6ihFvk4TvhmdfDTIM6cEONHUUdX4iCHuVxrIQbND8Z6YookZrTntr3EU9v3eQ%2FHZwhZjKANlNz3jc9Nd%2FFE4FuNPg3T0BW7bsKQGkaNJZ9dPXC39p2kzMWNqj4oMSp%2FV8u%2F9jz%2Fl8yLLaQB2L%2BeYsssxE2y9zlY7h%2Bnj1JT7UpROvVfwq543lFkfLz5e0hYlxkseKl0V%2F0t%2FjV%2Buw0etHJRF8Z7lhvqOP%2FiO0fUHQodGFsh6goGfXMLEGA6rqp2paJRjWxchakzDn%2BdHZtnpguIw2hDT4cqbTieC0ta91FT6sUuxehww5%2BXgyAY6pgFhaPIFCFwdQVaMoldCOGeoYvq8EggfWAczzL9%2FaX5IwfqH1XxGCiDYgDUj%2FIgH%2BHA%2FwSV%2Fceht29UKYZaRIcwEOHHoBfCuRn3WWs1ZhAUfRv%2B8aBrLPEOFo4eu0e5JLSRP3h8JdJXmvNuyahSh6mdui3QFGbrKEr2oafJzr%2FYo5HujL0zXcK5K6SintQLferPtW5PY22szI3QmNt869bcXU1zXznqp&X-Amz-Signature=aa7c02737a350b7ee7f547df21ddb59ff11ae976c193f381347d2ee8d6b87772&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

