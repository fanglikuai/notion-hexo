---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZEANCU%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKHel0Np7vx91I7eZYZiKqDaj%2BqMX3HKZ4hT9aBEL9%2FAIgAfLpGoxveRl8o%2FWoufqiGe%2FSwh3iiqhbwerlJRqme18q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDAPbfGWy7pKlldau7SrcA9Z6bPDXnin1PzcgqK2KDR%2FecT1mmfp9GxhGf14skV2twgWzjsrh8JJXaNThHd1WFdG15frGOVJOYdRzk%2FHRKa2DPiDUlK6zUjsjYBTtFVxGJBtNOkdx9TdSj%2BGc91ZG7db425QfYASdLMRqk3TWCHfnv8GUPm6BdsO76vt1EOhzEhzXLZ8edZTuNJheM0lqfnQqXnzUwmQlBRqP44fUZfF374B%2B1u4rKafZTH5LZXezYIX6IhFavrXVKyYkRqs7S%2ByBVSAzu7f2sNrZ%2BVaVBAvGLSF1B1WUJYHadz6G%2Bv2i0FPRplcUm6aT4JCR0GG9hr0Xf1IVvAqwlA7luQHiOo%2F2tSlaAcz69B2hnoa7Wmr9O2gP%2FwksNsG0rMhktAoiIk2c6At3HNP0wlEnfODqqInm%2BiViMGvTVsiKWixVGMC82TBZ19fSj3JUIypcpW2ubs2Mi9uHVAgVock1QVJd6xYBeidbmmgq8CYlOqdPbW7ca%2FuqbSVr20tg3nN6XcopdDe%2Fnt1bqpHZm25akXcHDAokpr1fKn%2BSyGjkGxGcdlGvCs97VxYpNUfquz8CaiqOswRSNAPJv6VXOOGNu3%2Fc1ZbREIOOj4eP3MUCKG%2FPG5x9v6GYp6kSEuQN3MWUMMDpwMYGOqUBqgEWp%2BGcepU%2BfsOW0WMFkIZBwJpCvKTVKWvS%2BztoYGP%2BddjZCrVC3t1lISpSx4XfkMdROId%2F%2Bma8aL7lqgNc70chnmntAs2wOMSSM0YYGWP2C29lZ2mMpNRWaoQPWnaHUQ3IQd2l7eggQ98q4FxV4TtGCEp8MfMT18u7HXRsvoWiVK8rYxqKPk4e6FluyjwrprAOiVAJgobhk6WS6SkySYhB1AWg&X-Amz-Signature=2631d6e954dab35b44f64f945f99ea6ad6bee7cd081ae52a138e4a9da2ae62bc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

