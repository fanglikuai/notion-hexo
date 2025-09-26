---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GKNT52U%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIAnQ1qmNjCIPLO%2BRKbR87YtsOqf5DA8erEHMF6e7I%2BUGAiAdyR3vwQOcaqcqVJJ2YAoP6zXEJ%2FMGEyglNh9GI1%2BgKSqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBvo2EYAaRGhU1XeuKtwDfphjbuYloK2pkZrySGxcEgboxEv%2FTdkf7fOO444MEURVyPurShIiMQoG6YBWOm%2B8LyoK9cGZ%2BGloQ2zflB1inNreLP3y4mJLv41LDb3tH5dT%2Bh0evUsAsqwjA9hOO1PztRlSs8tkI29at4WxX633aIKbGMa88XYTr2EtlAdPzswe5KDdGXWnEV9THlurCQv37qLMuF1BkRkfKZxjBe9nPoJseRfkF7KAiW54fjCbr5duW4s5lDvAf%2BXoCPozuqBjfsDFPJJdo%2FGJURUDkoSIPRd11qK%2Fwv3ymtvpNhjyFNuQLGVPZDHOdgbuXHPZdUKv%2BgqwBQhvbPZQchbh0GPDWeOOOkozio43L4FW%2BryAhpZiSTWqR3WlDOTJ6VF1%2BFuMXi8zxG45kNXnJzzgQu9DpL4VTBvO8YppfxUlCs%2BNVbqVj270m1Y87yCwgmQvBCRWEkbw8%2FBSMW9BbC70x1rgbq790pDHH0gpC8AYYRT02dqge5F6nlP0dQKGOj3Lc9v4oOo8EPN2vq244VlgbJI74RLSGDPKKeaYG44NFFhmr7oCRyTiKrbD7i5gAIhnGrRSaxkWelRH0LImXkCdLvPBl1BVNF279W0AfiuH0TsGz%2F8iTfWsb%2FLeEXPRKFEwmPfbxgY6pgEEiFDaRcKUv8UOfs33TzBsO7cLT2X%2FQtkkoCeFhLI2JTfkvWwINMzqyfHXTbIjqH4Uco%2FPIUFH%2FmHnH2J6kDOWSGnv3G5VwEoit%2BcJIv3hZ7CFFsD5dtOkqHYYHbwPgwe9iduo7zQpijrM4N%2FQMyz5Bzmebvd5H4eGVaHuaAEBZZZDhi2HQZZB7Yq1E7ncdttUJWn6xXyeVPfZqzjSryEae3OeRSqc&X-Amz-Signature=a0eaa0c5d420e694d1cf2c646a81b005bfd4700b64f72d7814d1734ff218b09b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

