---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2R4LNKY%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T170053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIAivNQUosGAaMF4jpd7ZzC21gPho3sUqObZq%2FCbY2ft9AiEA%2BDyHW%2FkXEO68CUWkoI%2B06ruzDqiGZzJ1CbwjbSe4PdcqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvJ%2FQ6s5C%2FMm0JMzircA2VyGzzY4i7sfmtXGfqxE1%2BO7A9BCfa3VJNd8fN9iD4XzzNXjN2VfjSmB42%2Br9IbwJ5weNMREUotmM%2FA4mXH4YoY0MOZjSULpid6ZVgI%2FogrJjNADqm%2Fzk5rV3%2BkKvqR7o%2BOvhEPFt2pEnGwuvPdTnwbhgOLXm7esJJya%2FyC%2FstjkVTE9184191hQYe01zuajtjy5CrFtrAHy%2B92q%2BNZIOJClUU%2FEhSVuT8Buk6DKfNg8TKeNmflR5kdrsAjlF7nbs7%2Fa9gbKoFSO%2B9R%2BD0g0MfbTeBH3QkopyLjsWh0oxLJrTjGScAbWsngiu2%2BeVy9iRpZojg1BJnjxgHEeK9EWUqJa7iwgUZEKEJG%2FrR8jgD26t7Cywat0A%2BJqhfdes4xWEG1UrLSqv%2BA6m4Eq3t1lRywzj6G6P3cmSEK85DuRiuhMz72inFcFNe%2FTnFHTjUwoqUHmJWibFVHEj8jxZ%2FgYLOte15d2lSApvjQCN2Ds1ZGPYF9TPVndwiuqozDTF00ThxXBBI7j8rk2lf8GVT8kE%2B8mx2Lr9sarMnkwsfHYPV8FoYKGjxqPlfUTrKy9nH7FMwrOxBDPTBaH%2F7myTj%2BJLL7WII7VSIQu%2Bso7didUV9stcMp7zIsb3Fjxj9yMLj2iMgGOqUB2a7uwjmnyn%2Fg%2FZIl0%2F4TpXp5vmwVTXNAiLjVxrJCiMlCn7OwdID8NITx7cBAkBCM3NnhqoPi%2BjC%2BPPvgE%2BBPBMxvXH%2Fk5x%2FRvNoInoOUVa%2Ftb%2BiW%2F6tl4zIf4dsfXbiq%2Ftz7wMC%2F6kvhQ3yi75ATQ9hYq4WRpd7gZgroSEvoWOQ2%2FRCECms3O0EyDUJK4m2EhMbeljoqsstf%2B1dQg959SId8TWUe&X-Amz-Signature=11c51eb7af457394f1b7dbd1ec4b3199e7cd6d71cef63e7ad26e2258d74543fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

