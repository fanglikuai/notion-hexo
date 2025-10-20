---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3RHRLAX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIEUuZilhdogSr6D1mVZlKJe1KEf%2BJKuxaJIFDuQu1fMuAiB5G%2Fsasfcny%2BTvAsdJ4jS1BUbdwesm%2Fk1dkb1SOUvr4CqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPQnYaev9BdhW8D7hKtwDcjvvAiZDiyYbAp2zyxQ921jU6LylnXrxhSoz2o4ZyPxLaEN4x8polasjF4xcCryfsq7ndRYDjjz0oyTBIAn1EQZ3SDHUjbW4JJZplU9dDf0%2B8HY1WHvOHrr7l6BTE%2FV%2FPuH8xPFB02egzu2AMQb47DGtI1SOfbJeJCoROXXYuKE%2FkmcQIG%2F9TuhXzuTzvkJXtE78aJKDgfKiJ1VE8SnAZBmDjCfmu9MvCcZff%2Btd2h%2BX9ZaxTPzuGi3U%2BhRwz%2FwE1TQuSbTWhEw48HAH7%2Bk%2BlcqDGNiQ6Z0twQ3H%2Fwo3wXQZMGAtv8xQjOSD5sBWgq%2BdZQ3KBlUuFVjWTRrAoZP9YEfll7nnq1f02YYRqSs5ssyYdF0aXaaXf8aNWD0ZevXi%2F3OS8BuHeskZSp0GYZp5tO%2BW1EM3urfAJazLveL%2FGpwtNNjYgmBy15i9kl0AyOLHSYkpgjTlpXlbqsVNHU07LI0PEdYRAxBrKyniaAaLNM%2F2%2F6%2BVr7HT1H0YJMLQtcPBHu%2F3Rn%2F%2F56cEjL6xWyA6aeKN0PYBA0BHMAGZRh91DqdGuxZkQSuGWcMAStxl8g45xW3beAU7pZmJBi1S2JWZkWH%2FwowIGTQfwvBQF3QX3xxUFw795J0lYSg6YHAwmdfUxwY6pgH5jayAeBrVEGSvn541Lmdl%2BN%2BWtAbPZDu0S%2FsBwXIIfVA1pHsPQ2NF3pmYBZaYUpS%2F2ZLnycZAK8d0Af0mE7G5C5jIchi%2FYw6JQ7OdJfpGZZPpD8wjCSTJvUDTOJWQp5pO2dg%2Bktw7dl9jJcS0j4b5hoYvRr9imTYXlO20AySpM1LjmInJN2g8uNYZ60QUl11f49E5y8K%2BGbppYOQk22p%2FXczzmea7&X-Amz-Signature=330b9ba6e585a742ff9cc8cf5d76e996065c00393563c559d96ef568d159a47f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

